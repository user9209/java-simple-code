# transferTo

```
InputStream is = System.in;
OutputStream os = System.out;
is.transferTo(os);
```
## selfmade

```
InputStream is = System.in;
OutputStream os = System.out;
transferTo(is,os);

public static void transferTo(InputStream is, OutputStream os) throws IOException {
    byte[] buf = new byte[4096];
    int l;

    while ((l = is.read(buf)) != -1) {
        os.write(buf, 0, l);
        os.flush();
    }
}
```
