# Lab

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages

## Payload

Ici on a juste a faire pop une erreur pour savoir quel framework est utilisé.

On peut aller sur la page d'un produit et mettre un identifiant qui n'existe pas `https://0acf00de0468d97181d1c74500f600ff.web-security-academy.net/product?productId=ssss`

```
Internal Server Error: java.lang.NumberFormatException: For input string: "s"
	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
	at java.base/java.lang.Integer.parseInt(Integer.java:668)
	at java.base/java.lang.Integer.parseInt(Integer.java:786)
	at lab.b.u.v.t.a(Unknown Source)
	at lab.s.k.n.s.g(Unknown Source)
	at lab.s.k.h.c.j.g(Unknown Source)
	at lab.s.k.h.y.lambda$handleSubRequest$0(Unknown Source)
	at e.l.q.s.lambda$null$3(Unknown Source)
	at e.l.q.s.p(Unknown Source)
	at e.l.q.s.lambda$uncheckedFunction$4(Unknown Source)
	at java.base/java.util.Optional.map(Optional.java:260)
	at lab.s.k.h.y.t(Unknown Source)
	at lab.server.a.p.z.L(Unknown Source)
	at lab.s.k.d.l(Unknown Source)
	at lab.s.k.d.L(Unknown Source)
	at lab.server.a.p.e.j.k(Unknown Source)
	at lab.server.a.p.e.w.lambda$handle$0(Unknown Source)
	at lab.b.d.u.i.V(Unknown Source)
	at lab.server.a.p.e.w.O(Unknown Source)
	at lab.server.a.p.o.b(Unknown Source)
	at e.l.q.s.lambda$null$3(Unknown Source)
	at e.l.q.s.p(Unknown Source)
	at e.l.q.s.lambda$uncheckedFunction$4(Unknown Source)
	at lab.server.re.F(Unknown Source)
	at lab.server.a.p.o.U(Unknown Source)
	at lab.server.a.h.o.R(Unknown Source)
	at lab.server.a.q.D(Unknown Source)
	at lab.server.a.c.D(Unknown Source)
	at lab.server.rw.d(Unknown Source)
	at lab.server.rw.Z(Unknown Source)
	at lab.k.q.lambda$consume$0(Unknown Source)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
	at java.base/java.lang.Thread.run(Thread.java:833)

Apache Struts 2 2.3.31
```

Framework : `Apache Struts 2 2.3.31`