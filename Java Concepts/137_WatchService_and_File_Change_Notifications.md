# `WatchService` (NIO.2) for File Change Notifications

**`WatchService`** watches directories for **create/modify/delete** events—useful for **hot reload**, **config file** updates, and **ETL drop folders** on the JVM.

---

## Sketch

```java
try (WatchService ws = FileSystems.getDefault().newWatchService()) {
  Path dir = Path.of("config");
  dir.register(ws, StandardWatchEventKinds.ENTRY_MODIFY);
  WatchKey key = ws.take(); // blocking
  for (WatchEvent<?> ev : key.pollEvents()) { /* handle */ }
  key.reset();
}
```

---

## Pitfalls

- **Platform semantics** differ (macOS coalescing, polling backends).
- **Not** a distributed file watcher—use **message bus** for cluster-wide changes.

---

## Related

- [NIO.2](48_NIO2.md)
- [File I/O](20_File_IO.md)
