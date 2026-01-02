# BROKER MQ REQUEST REPLY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Request Reply (pattern of EIP)

producer'ın attığı mesajın içinde, consumer'ın hangi topic'e response olarak mesaj atması gerektiği de yazmaktadır. Bu adres `Return Address`'tir.

Hangi mesaja reply yapıldığı `correlation ID` üzerinden kontrol edilir.

## 📌broker native support

Aslında JMS'in burada desteği sadece giden mesajın formatıdır.

Aslıda aşağıdaki JMS kodu incelenediğinde şunu görürüz: Birden fazla producer'ın instance'ı varsa mesaj diğer instance'lara düşebilir.

Bunu engellemek için:
- `JSM`'te `TemporaryQueue` kullanılır. Her request'te, reply topiği temporary yollanır ve bu `TemporaryQueue`'yu sadece ilgili instance follow eder.
- Benzer çözüm `Kafka`'da da uygulanır. Fakat bunun için hazır API yok.

Eğer bu çözümü uygularsak aynı thread alında `HTTP` isteği atmış ve cevap almış gibi kod yazabiliriz. Bunun `HTTP`'ye göre:

- `avantajı`: istekler consumer işi bittikçe işlenecektir, ama `HTTP` gibi basit kod yazabiliriz. Consumer'un backpressure yapmasına gerek kalmaz. Çünkü zaten işi bittikçe mesaj pull edecek.
- `Dezavantajı`: broker kullanıldığı için sistem kaynakları tüketilecek.

`GRPC`'deki backpressure ile her iki dezavantajı da ortadan kaldırmış ve sorunu çözmüş oluyoruz.

## 📌 JSM kod örneği

Bu örnek multi instance uygulamada sağlıklı çalışmaz.

https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReplyJmsExample.html

Kopyası (constructor'ı ve initialiser'ı sadeleştirdim biraz):

requester kodu:

```java
import javax.jms.Connection;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageConsumer;
import javax.jms.MessageProducer;
import javax.jms.Session;
import javax.jms.TextMessage;
import javax.naming.NamingException;

public class Requestor {

	private Session session;
	private Destination replyQueue;
	private MessageProducer requestProducer;
	private MessageConsumer replyConsumer;
	private MessageProducer invalidProducer;

	protected void initialize(Connection connection, 
                            String requestQueueName, 
                            String replyQueueName, 
                            String invalidQueueName)
                            throws NamingException, JMSException {
			
		session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		
		Destination requestQueue = "requestQueueName";
		replyQueue = "replyQueue";
		Destination invalidQueue = "invalidQueueName";
		
		requestProducer = session.createProducer(requestQueue);
		replyConsumer = session.createConsumer(replyQueue);
		invalidProducer = session.createProducer(invalidQueue);
	}

	public void send() throws JMSException {
		TextMessage requestMessage = session.createTextMessage();
		requestMessage.setText("Hello world.");
		requestMessage.setJMSReplyTo(replyQueue);
		requestProducer.send(requestMessage);
		System.out.println("Sent request");
		System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
		System.out.println("\tMessage ID: " + requestMessage.getJMSMessageID());
		System.out.println("\tCorrel. ID: " + requestMessage.getJMSCorrelationID());
		System.out.println("\tReply to:   " + requestMessage.getJMSReplyTo());
		System.out.println("\tContents:   " + requestMessage.getText());
	}

	public void receiveSync() throws JMSException {
		Message msg = replyConsumer.receive();
		if (msg instanceof TextMessage) {
			TextMessage replyMessage = (TextMessage) msg;
			System.out.println("Received reply ");
			System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
			System.out.println("\tMessage ID: " + replyMessage.getJMSMessageID());
			System.out.println("\tCorrel. ID: " + replyMessage.getJMSCorrelationID());
			System.out.println("\tReply to:   " + replyMessage.getJMSReplyTo());
			System.out.println("\tContents:   " + replyMessage.getText());
		} else {
			System.out.println("Invalid message detected");
			System.out.println("\tType:       " + msg.getClass().getName());
			System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
			System.out.println("\tMessage ID: " + msg.getJMSMessageID());
			System.out.println("\tCorrel. ID: " + msg.getJMSCorrelationID());
			System.out.println("\tReply to:   " + msg.getJMSReplyTo());

			msg.setJMSCorrelationID(msg.getJMSMessageID());
			invalidProducer.send(msg);

			System.out.println("Sent to invalid message queue");
			System.out.println("\tType:       " + msg.getClass().getName());
			System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
			System.out.println("\tMessage ID: " + msg.getJMSMessageID());
			System.out.println("\tCorrel. ID: " + msg.getJMSCorrelationID());
			System.out.println("\tReply to:   " + msg.getJMSReplyTo());
		}
	}
}
```

Consumer kodu:

```java
import javax.jms.Connection;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageConsumer;
import javax.jms.MessageListener;
import javax.jms.MessageProducer;
import javax.jms.Session;
import javax.jms.TextMessage;
import javax.naming.NamingException;

public class Replier implements MessageListener {

	private Session session;
	private MessageProducer invalidProducer;

	protected void initialize(Connection connection,
                            String requestQueueName,
                            String invalidQueueName)
		                        throws NamingException, JMSException {

		session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		Destination requestQueue = "requestQueueName";
		Destination invalidQueue = "invalidQueueName";

		MessageConsumer requestConsumer = session.createConsumer(requestQueue);
		MessageListener listener = this;
		requestConsumer.setMessageListener(listener);

		invalidProducer = session.createProducer(invalidQueue);
	}

	public void onMessage(Message message) {
		try {
			if ((message instanceof TextMessage) && (message.getJMSReplyTo() != null)) {
				TextMessage requestMessage = (TextMessage) message;

				System.out.println("Received request");
				System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
				System.out.println("\tMessage ID: " + requestMessage.getJMSMessageID());
				System.out.println("\tCorrel. ID: " + requestMessage.getJMSCorrelationID());
				System.out.println("\tReply to:   " + requestMessage.getJMSReplyTo());
				System.out.println("\tContents:   " + requestMessage.getText());

				String contents = requestMessage.getText();
				Destination replyDestination = message.getJMSReplyTo();
				MessageProducer replyProducer = session.createProducer(replyDestination);

				TextMessage replyMessage = session.createTextMessage();
				replyMessage.setText(contents);
				replyMessage.setJMSCorrelationID(requestMessage.getJMSMessageID());
				replyProducer.send(replyMessage);

				System.out.println("Sent reply");
				System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
				System.out.println("\tMessage ID: " + replyMessage.getJMSMessageID());
				System.out.println("\tCorrel. ID: " + replyMessage.getJMSCorrelationID());
				System.out.println("\tReply to:   " + replyMessage.getJMSReplyTo());
				System.out.println("\tContents:   " + replyMessage.getText());
			} else {
				System.out.println("Invalid message detected");
				System.out.println("\tType:       " + message.getClass().getName());
				System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
				System.out.println("\tMessage ID: " + message.getJMSMessageID());
				System.out.println("\tCorrel. ID: " + message.getJMSCorrelationID());
				System.out.println("\tReply to:   " + message.getJMSReplyTo());

				message.setJMSCorrelationID(message.getJMSMessageID());
				invalidProducer.send(message);

				System.out.println("Sent to invalid message queue");
				System.out.println("\tType:       " + message.getClass().getName());
				System.out.println("\tTime:       " + System.currentTimeMillis() + " ms");
				System.out.println("\tMessage ID: " + message.getJMSMessageID());
				System.out.println("\tCorrel. ID: " + message.getJMSCorrelationID());
				System.out.println("\tReply to:   " + message.getJMSReplyTo());
			}
		} catch (JMSException e) {
			e.printStackTrace();
		}
	}
}
```