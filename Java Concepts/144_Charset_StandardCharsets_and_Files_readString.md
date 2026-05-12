# `Charset`, `StandardCharsets`, and `Files.readString`

**`Charset`** names encoding rules; **`StandardCharsets.UTF_8`** avoids **`Charset.forName`** typos. **`Files.readString(Path)`** reads text with an explicit charset—**UTF-8** should be the **default** choice for new IO.

---

## Pitfalls

- **`new String(bytes)`** uses **platform default** charset—**non-reproducible** across servers.
- **Mixed** encodings in pipelines cause **mojibake**—normalize at **boundaries**.

---

## HTTP

**`java.net.http`** and servlet stacks should declare **`UTF-8`** for JSON bodies explicitly when headers absent.

---

## Related

- [File I/O](20_File_IO.md)
- [NIO.2](48_NIO2.md)
