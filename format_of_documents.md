
# FORMAT/STANDARDS OF DOCUMENTS

All tutorials must follow all below standards.

## 📌 TEXT (CONTENT)

- It's suggested to split paragraphs or text into items. This makes it easier to read and update the sentences.

- All sentences must be short and simple. `Simple English` language of `Wikipedia` can be taken reference.

- There are many `Markdown` languages (as it is explained on `Markdown` topic inside tutorials). In this project, we use only `GitHub Flavored Markdown (or GFM)`, which is supported by `github.com` and `VSCode` by default.

- `Markdown` allows `HTML` and `CSS`. However we don't use them. Because `github.com` ignores them (and other `Markdown` viewers may ignore them).

- Do not use `>` for code blocks. Always ` ``` ` must be used even for single-line code.

- Do not break the syntax of the programming language as shown in the example below:

  ```java
  void methodA(){
    ...
  }
  ```

  The following must be used:

  ```java
  void methodA(){
    // other code here
  }
  ```

- Do not use words that are not validated by spell-checker.

  Example:

  ```java
  void methodT(){
    
  }
  ```

  The following must be used:

  ```java
  void methodTest(){

  }
  ```

- All files are in `UTF-8` format. But avoid using every `UTF-8` supported characters. Test each character in different input UI components before use it. This is because some characters (especially icons) are not supported by many document viewers/editors. As an example `▇` (full box) character is supported by many viewers/editors. But the length is may different across different viewer/editor. Therefore prefer `#` instead of `▇`.

## 📌 QUOTING (USING DOUBLE QUOTATION MARKS) & BOLD & ITALIC

All rules in this title must be followed only for paragraphs.

That means below blocks should not be follow the rules in this title.

- comments within the source code
- title (any level)

### 📌📌 RULE 1

- `"` must not be used.

  Example wrong usage:

  ```markdown
  "Spring boot" is a Java framework.
  ```

- `'` must be only used as apostrophe.

  Example wrong usage:

  ```markdown
  'Spring boot' is a Java framework.
  ```

#### 📌📌📌 Reason for this rule

We already wrap all special text using `` ` ``. This will be standardized.

### 📌📌 RULE 2

`Bold` must not be used. Instead the text can be wrapped via `` ` ``.

#### 📌📌📌 Reason for this rule

In the future I may use bold for spesific cases only.

### 📌📌 RULE 3

`Italic` must not be used. Instead the text can be wrapped via `` ` ``.

#### 📌📌📌 Reason for this rule

Italic font can not be easily distinguished on most `Markdown`-viewer.

### 📌📌 RULE 4

A text (phrase or expression) must be wrapped via `` ` `` only if meets the below conditions.

- If the text is proper name (brand, company name, city name, library name, person name etc).

   Correct:

  ```markdown
  `England` is on `Europe`.
  ```

- If the text is a technical term.

  But for the below terms (and their derivatives and their abbreviations) we must not follow this rule.

  - database
  - server
  - service
  - log
  - code
  - source
  - repository
  - repo
  - IDE
  - class
  - library
  - framework
  - login
  - logout
  - network
  - plugin
  - extension
  - format

  Correct:

  ```markdown
  One `cluster` has multiple `node` instances.
  ```

- If the text is a mathematical variable name.

- If the text is a programming language variable name.

- If the text contains any character except:

  - a-Z

  - 0-9

  - dash character without space before or after

  Correct:

  ```markdown
  Some characters like `?` is important.
  ```

  Correct:

  ```markdown
  Start with `-Dparam1` VM parameter.
  ```

  Correct:
  
  ```markdown
  The new syntax is human-readable.
  ```

#### 📌📌📌 Reason for this rule

- It will be more human-readable
- spell checker plugin knows that it's a special term.

### 📌📌 RULE 5

A text may have abbreviations or synonyms. But:

- abbreviations and synonyms must only be mentioned once in the same sentence throughout the entire project.
- we must use the shortest one on rest of the whole repository.

#### 📌📌📌 Example

The abbreviation of `Operating System` is `OS`. We must only use `OS` in whole repository because it is the shortest one. But we must write only once a sentence like below one:

```markdown
`Operating System (or OS)` run other process.
```

#### 📌📌📌 Reason for this rule

If the user (who reads the tutorials) selects the term (generally via double-click via mouse)

  - he can search that term quickly where else it is used.
  - viewer (software) highlights the term automatically if it is used elsewhere on the same document.

We use shortest because:

  - Display more information on smaller screens.
  - Take up less space (file size).

### 📌📌 RULE 6

Some terms can be used via dash or white space.

- `Spring boot`
- `Spring-boot`

For those cases only one style of that term must be used throughout the entire repository.

#### 📌📌📌 Reason for this rule

Same reasons with `Rule 5`.

## 📌 FILE SIZE

Each file must be lower than 400 KB.

### 📌📌 Reason for this rule

- `github.com` does not show `Markdown` output (preview) for big files.
- Better performance when editing or reading files (especially from mobile/tablets).
- Some markdown viewers do not show `Title of content` for file which has too many titles.

## 📌 REFERENCES (SOURCES)

- There may be missing information (it will always be there anyway), but the accuracy of the available information is very important. If we are not sure about the accuracy of a sentence, it must be mentioned that we have no any sources related to it.

- The source of every simple sentence does not need to be specified in order to prevent the file from becoming too large.

- Each URL (even they are not source of any content) must be archived/cached on `archive.is` and `web.archive.org`. Also they must backup locally as `PDF` and `HTML` format. The details (like archive date) must be given in [file:"sources.yml"](./sources.yml) file.

- The image files must be saved as PDF only. There is no reason to save `PDF` or `image` files in `HTML` format. In those cases, the source must be flag as `no_need_html_backup_of_pdf_or_image_files` on [file:"sources.yml"](./sources.yml) file.

- All the details about the source file (like title, subtitle), must be written directly inside articles. If the source file is video and the video player page does not contain extra information, then the page must be flag as `video_only_and_no_other_information_to_backup` on [file:"sources.yml"](./sources.yml) file.

- If the source file size is too big and it does not contain important data then it must not be backup locally. Those sources must be marked as `content_is_not_important_and_too_big`.

- Source code or any other text output must also be saved in multiple formats locally. Because of below reasons:
  - In some cases text copying is buggy from `PDF` files. In this cases we may need `HTML` file.
  - In some cases we may need to search in multiple source files via text based search engines like `grep`. In this case, we will need `HTML` file (which is text based).
  - We may also need `PDF` files in long term. Because some `HTML` files (which is saved today) may not open %100 properly in the future (after 10 year later). There are no backward compatibility standards for web browsers.

- When searching already archived URL's from `archive.is` or `web.archive.org` all below combinations should be try:
  - with and without `hashtag` (`#`)
  - `http` or `https`

- Source ID's are immutable. They can be deleted but not updated. That means, if the number is deleted, it can never be used again. (Otherwise they can be mixed in local backups).

## 📌 EXPORT

Do not export/convert files as `PDF` (`A4`) or any portrait-oriented file format.

Prefer `HTML` because it supports landscape tables.

### 📌📌 Reason for this rule

The large tables do not fit on the page.

## 📌 TITLE ICON AND LEVELS

Most frequently Markdown is rendering to `HTML`. `HTML` accepts only one `H1` item. Therefore only one `H1` title must be use on a file.

All titles must include 📌 (pin) icon depending on its level. Because many HTML does not render the sub-titles very clear (example: they are not bold enough). Also the end user cannot understand which level the subheading is.

- 1st level title must not have any 📌. (This is the file name)

- 2nd level title must have 1 📌.

- 3rd level title must have 2 📌.

- 4rd level title must have 3 📌.

- If you need 5th subtitle please consider to separate the topics.

No pin or similar icon should ever be used anywhere other than titles.

## 📌 TITLE FORMAT

- Abbreviations must have a prefix `abb-tr`, `abb-eng`. Abbreviations must be written in the title. That makes easier to search with keywords. But the prefixes are optional. Prefixes (such as `abb-eng`) does not have to be added if not necessary.
  - `Example-1`: `app` is abbreviation of `application`. This information is well known.
  - `Example-2`: If the title has `Greek` characters then we don't need `gr` prefix.

  Valid title examples:

  - > eng:application (or abb-eng:app or tr:uygulama or gr:πρόγραμμα)

  - > application (or app or tr:uygulama or gr:πρόγραμμα)

  - > application (or app or tr:uygulama or πρόγραμμα)

- If an abbreviation is abbreviation of a proper name (independent of any language) it gets a prefix only: `abb`. Example:

  > Kubernetes (or abb:K8s)

- If a title has an older synonym, it gets `old-name` prefix. Example:

  > Solaris (or old-name:SunOS)

  If a title has an older name, that means the term is proper name. And proper names are independent of any language. Therefor it does not get a language keyword (lile `eng`, `tr`, `gr`).

- Any information about the title terminology must be written in the first sentence after the title.

- Title must contain exact synonym words. The words in the title must not have close meanings to each other. Otherwise it can be mentioned in the next line.

- If the text which has synonym or abbreviation it should be mention on title. If that term does not appear in any title, only then we can use it inside paragraph as below:

  > Firefox is an open source `application (or abb-eng:app or tr:uygulama)`.

- Version lists/tables must have a title which suffixed with `version history`. Example:

  - > HTTP version history

  - > Android version history

  This makes searching standardized.
