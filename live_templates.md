A list of the live templates I am using.
They are [Apache Velocity templates](https://velocity.apache.org/engine/2.3/vtl-reference.html), and variables are
defined by a single function invocation. The list of supported functions is given
in [JetBrains documentation](https://www.jetbrains.com/help/idea/using-live-templates.html).

# For Java

### @c

<dl>
<dt>Description</dt>
<dd>Javadoc @code</dd>
<dt>Applicable in Java</dt>
<dd>comment</dd>
<dt>Template text</dt>
</dl>

```java
{@code $VAL$}$END$
```

### @f

<dl>
<dt>Description</dt>
<dd>Javadoc {@code false}</dd>
<dt>Applicable in Java</dt>
<dd>comment</dd>
<dt>Template text</dt>
</dl>

```java
{@code false}
```

### @l

<dl>
<dt>Description</dt>
<dd>Javadoc @link</dd>
<dt>Applicable in Java</dt>
<dd>comment</dd>
<dt>Template text</dt>
</dl>

```java
{@link $VAL$}$END$
```

### @n

<dl>
<dt>Description</dt>
<dd>Javadoc {@code null}</dd>
<dt>Applicable in Java</dt>
<dd>comment</dd>
<dt>Template text</dt>
</dl>

```java
{@code null}
```

### @t

<dl>
<dt>Description</dt>
<dd>Javadoc {@code true}</dd>
<dt>Applicable in Java</dt>
<dd>comment</dd>
<dt>Template text</dt>
</dl>

```java
{@code true}
```

### checkpara

<dl>
<dt>Description</dt>
<dd>Checks the method parameters</dd>
<dt>Applicable in Java</dt>
<dd></dd>
<dt>Template text</dt>
</dl>

```java
$content$
```

<dl>
<dt>Variable: content</dt>
</dl>
```groovy
groovyScript("def params = _1.collect { 'java.util.Objects.requireNonNull(' + it + ', \"' + it + ' shall not be null in ' + _2 + '()\");' }.join('\\n'); params", methodParameters(), methodName());
```

### copy

<dl>
<dt>Description</dt>
<dd>Create the HAPI copy() method</dd>
<dt>Applicable in Java</dt>
<dd>declaration</dd>
<dt>Variable: CLASS</dt>
<dd><code>className()</code></dd>
<dt>Template text</dt>
</dl>

```java
@Override
public $CLASS$ copy() {
  final var copy = new $CLASS$();
  this.copyValues(copy);
  return copy;
}
```

### copyValues

<dl>
<dt>Description</dt>
<dd>Create the HAPI copyValues() method</dd>
<dt>Applicable in Java</dt>
<dd>declaration</dd>
<dt>Variable: CLASS</dt>
<dd><code>className()</code></dd>
<dt>Template text</dt>
</dl>

```java
@Override
public void copyValues(final $CLASS$ dst) {
  super.copyValues(dst);
  if (dst instanceof final $CLASS$ als) {
      als.status = status == null ? null : status.copy();
  }
}
```

### logjava

<dl>
<dt>Description</dt>
<dd>Insert a logger (Java)</dd>
<dt>Applicable in Java</dt>
<dd>declaration</dd>
<dt>Variable: CLASS</dt>
<dd><code>className()</code></dd>
<dt>Template text</dt>
</dl>

```java
private static final java.util.logging.Logger LOG = java.util.logging.Logger.getLogger("$CLASS$");
```

### logslf4j

<dl>
<dt>Description</dt>
<dd>Insert a logger (SLF4J)</dd>
<dt>Applicable in Java</dt>
<dd>declaration</dd>
<dt>Variable: CLASS</dt>
<dd><code>className()</code></dd>
<dt>Template text</dt>
</dl>

```java
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger($CLASS$.class);
```

### main

<dl>
<dt>Description</dt>
<dd>main() method declaration</dd>
<dt>Applicable in Java</dt>
<dd>declaration</dd>
<dt>Template text</dt>
</dl>

```java
public static void main(String[] args){
  $END$
}
```

### setpara

<dl>
<dt>Description</dt>
<dd>Sets the method parameters</dd>
<dt>Applicable in Java</dt>
<dd></dd>
<dt>Template text</dt>
</dl>

```java
$content$
```

<dl>
<dt>Variable: content</dt>
</dl>
groovyScript("def params = _1.collect { 'this.' + it + ' = java.util.Objects.requireNonNull(' + it + ');' }.join('\\n'); params", methodParameters());
