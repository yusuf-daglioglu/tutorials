
# FORMAT/STANDARDS OF DOCUMENTS

All tutorials must follow all below standards.

## 📌 TEXT (CONTENT)

- Splitting any paragraph/text in to items is suggested. That makes easier to read and update the sentences.

- All explanations/sentences must be short and simple. Wikipedia's "Simple English" language can be taken reference.

- There are many Markdown languages (as it is explained on "Markdown" topic inside tutorials). In this project we use only "`GitHub Flavored Markdown (or GFM)`" which is being supported by "`github.com`" and VSCode as default.

- Bold words must be defined with double underscore character. (Even both CommonMark and GFM supports using double star as well)

- Markdown allows HTML and CSS. However we don't use them. Because "`github.com`" ignores them (and other Markdown-viewers may ignore them).

- Do not use `>` for code blocks. Always ` ``` ` must be used even for single-line code.

- Do not break the syntax of the programming language as below example:

  ```java
  void methodA(){
    ...
  }
  ```

  Below one must be used:

  ```java
  void methodA(){
    // other code here
  }
  ```

- Do not use words which are not validated by spell-checker:

  ```java
  void methodA(){

    // don't use this:
    print("asdsad");

    // below one must be used:
    print("hello");
  }
  ```

- All files are in UTF-8 format. But do not use all UTF-8 supported characters. Test each character in different input UI components before use it. Because some characters (especially icons) are not supporting by many document viewers/editors. As an example "`▇`" (full box) character is supporting by many viewers/editors. But the length is different on different viewer/editor. Therefore prefer "`#`" instead of "`▇`".

- Special term rules explained below.

  ```markdown
  Valid and should be prefered:
  "`ABC Service`"

  Valid but not prefered:
  "ABC Service"

  Must not be used:
  'ABC Service'
  ```
  
  If a term contains any character except:

  ```
  a-Z
  
  0-9
  
  - (without space before or after)
  ```

  Use below examples for syntax:

  ```markdown
  "`myDomain.com`"

  "`ABC+ Service`"

  Question mark "`?`" character is important.
  ```

  And don't use `'` or `"` side by side. Example:

  ```markdown
  Dont't write this:

  "`ABC+ Service`"'s port is 8081.


  Below one must be used:

  Port of "`ABC+ Service`" is 8081.
  ```

## 📌 FILE SIZE

Each file must not be greater than 400 KB. Because of 2 following reasons:
  - "`github.com`" does not show Markdown output (preview) files which are greater than 1 MB.
  - Better performance when editing and reading files (especially from mobile/tablets).
  - Some markdown viewers do not show files which has more then 50 titles.

## 📌 REFERENCES (SOURCES)

- There may be missing information (it will always be there anyway), but the accuracy of the available information is very important. If we are not sure about the accuracy of a sentence, it must be mentioned that we have no any sources related to it.

- The source of every simple sentence must not be specified to prevent the file to be big.

- Each URL (even they are not source of any content) must be archived/cached on "archive.is" and "web.archive.org". Also they must backup locally as PDF and HTML format. The details (like archive date) must be given in [file:"sources.yml"](./sources.yml) file.

- The image files must be saved as PDF only. There is no reason to save "PDF" or "image" files in HTML format. In those cases, the source must be flag as "`no_need_html_backup_of_pdf_or_image_files`" on [file:"sources.yml"](./sources.yml) file.

- All the details about the source file (like title, subtitle), must be written directly inside articles. If the source file is video and the video player page does not contain extra information, then the page must be flag as "`video_only_and_no_other_information_to_backup`" on [file:"sources.yml"](./sources.yml) file.

- If the source file size is too big and it does not contain important data then it must not be backup locally. Those sources must be marked as "`content_is_not_important_and_too_big`".

- Source code or any other text output must saved also in multiple formats locally. Because of two reasons:
  - In some cases text copying is buggy from PDF files. In this cases we may need HTML file.
  - In some cases we may need to search in multiple source files via text based search engines like "grep". In this case, we will need HTML file (which is text based).
  - We may also need PDF files in long term. Because some HTML files (which is saved today) may not open %100 properly in the future (after 10 year later). There is no browser backward compatibility standards for web browsers.

- When searching already archived URL's from "archive.is" or "web.archive.org" all below combinations should be try:
  - with and without hashtag (#)
  - http or https

- Source ID's are immutable. They can be deleted but not updated. That means, if the number deleted, it can never be used again. (Otherwise they can be mixed in local backups).

## 📌 EXPORT

Do not export/convert files as PDF (A4) or any landscape file format. Because the tables does not fit on the page. Prefer "HTML" which supports landscape tables.

## 📌 TITLE ICON AND LEVELS

Markdown rendered most frequently to HTML. HTML accepts only one H1 item. Therefore only 1 H1 title must be use on a file.

All titles must include 📌 (pin) icon depending on its level. Because many HTML does not render the sub-titles very clear (example: they are not bold enough). Also the end user cannot understand which level the subheading is.

- 1st level title must not have any 📌. (This is the file name)

- 2nd level title must have 1 📌.

- 3rd level title must have 2 📌.

- 4rd level title must have 3 📌.

- If you need 5th subtitle please consider to separate the topics.

Any pin or similar icon must never be used anywhere else except titles.

## 📌 TITLE FORMAT

- Abbreviations must have a prefix "abb-tr", "abb-eng". Abbreviations must be written in the title. That makes easier to search with keywords. But the prefixes are optional. Prefixes (such as "abb-eng") does not have to be added (if not necessary).
  - Example-1: "app" is abbreviation of "application". This information is well known.
  - Example-2: If the title has Greek characters then we don't need "gr" prefix.

  Valid title examples:

  - > eng:application (or abb-eng:app or tr:uygulama or gr:πρόγραμμα)

  - > application (or app or tr:uygulama or gr:πρόγραμμα)

  - > application (or app or tr:uygulama or πρόγραμμα)

- If an abbreviation is abbreviation of a proper name (independent of any language) it gets a prefix only: "abb". Example:

  > Kubernetes (or abb:K8s)

- If a title has an older synonym, it gets "old-name" prefix. Example:

  > Solaris (or old-name:SunOS)

  If a title has an older name, that means the term is proper name. And proper names are independent of any language. Therefor it does not get a language keyword (lile eng, tr, gr).

- Any information about the title terminology must be written in the first sentence after the title.

- Title must contain exact synonym words. The words in the title must not have close meanings to each other. Otherwise it can be mentioned in the next line.

- In order to define the word within the paragraph, it needs to be defined same as in title format. Example:

  > Firefox is an open source application (or abb-eng:app or tr:uygulama)

- Version lists/tables must have a title which suffixed with "version history". Example:

  - > "HTTP version history"
  - > "Android version history"

  This makes searching standardized.
