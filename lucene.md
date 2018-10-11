### Quy trình tạo chỉ mục

- Tạo document:

  - Tạo đối tượng Document()

  ```Java
  Document document = new Document();
  ```

  - Tạo đối tượng Field()

  ```Java
  Field pathField = new StringField("path", file.toString(), Field.Store.YES);
  ```

  - ```add``` các field vào document

  ```Java
   document.add(filePathField);
  ```

- Tạo IndexWriter:

  - Tạo đối tượng lucene Directory 

  ```Java
  Directory dir = FSDirectory.open(Paths.get(indexPath));
  ```

  - Tạo đối tượng IndexWriter với tham số đầu vào là directory trên

  ```Java
  IndexWriter writer = new IndexWriter(dir, iwc);
  ```

- Bắt đầu đánh chỉ mục

```Java
writer.addDocument(document);
```

