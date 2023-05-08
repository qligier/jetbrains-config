# Java

They are [Apache Velocity templates](https://velocity.apache.org/engine/2.3/vtl-reference.html).

### QGetter

```java
#if ($field.modifierStatic)
static ##
#end
$field.type ##
#if($field.recordComponent)
  ${field.name}##
#else
#set($name = $StringUtil.capitalizeWithJavaBeanConvention($StringUtil.sanitizeJavaIdentifier($helper.getPropertyName($field, $project))))
#if ($field.boolean && $field.primitive)
  is##
#else
  get##
#end
${name}##
#end
() {
  return this.$field.name;
}
```

For `private String field`, it generates:

```java
public String getField() {
    return this.field;
}
```

### QSetter

```java
#set($paramName = $helper.getParamName($field, $project))
#if($field.modifierStatic)
static ##
#end
void set$StringUtil.capitalizeWithJavaBeanConvention($StringUtil.sanitizeJavaIdentifier($helper.getPropertyName($field, $project)))(final $field.type $paramName) {
  #if ($field.name == $paramName)
    #if (!$field.modifierStatic)
      this.##
    #else
      $classname.##
    #end
  #end
  $field.name = $paramName;
}
```

For `private String field`, it generates:

```java
public void setField(final String field) {
    this.field = field;
}
```

### Q java.util.Objects.equal and hashCode JDK17

Equals Template:

```java
#parse("equalsHelper.vm")
public boolean equals(##
#if ($settings.generateFinalParameters)
  final ##
#end
Object $paramName){
  if (this == o) return true;
  if (!($paramName instanceof final $classname that)) return false;
  #if ($superHasEquals)
    if (!super.equals(o)) return false;
  #end
  return ##
   #set($i = 0)
   #foreach($field in $fields)
     #if ($i > 0)
     
       && ##
     #end
     #set($i = $i + 1)
     #if ($field.primitive)
       #if ($field.double || $field.float)
         #addDoubleFieldComparisonConditionDirect($field) ##
       #else
         #addPrimitiveFieldComparisonConditionDirect($field) ##
       #end
     #elseif ($field.enum)
       #addPrimitiveFieldComparisonConditionDirect($field) ##
     #elseif ($field.array)
java.util.Arrays.equals($field.accessor, ${classInstanceName}.$field.accessor)##
     #elseif ($field.notNull)
${field.accessor}.equals(${classInstanceName}.$field.accessor)##
     #else
java.util.Objects.equals($field.accessor, ${classInstanceName}.$field.accessor)##
     #end
   #end
  ;
}
```

HashCode Template:

```java
public int hashCode() {
#if (!$superHasHashCode && $fields.size()==1 && $fields[0].array)
  return java.util.Arrays.hashCode($fields[0].accessor);
#else
  #set($hasArrays = false)
  #set($hasNoArrays = false)
  #foreach($field in $fields)
    #if ($field.array)
      #set($hasArrays = true)
    #else
      #set($hasNoArrays = true)
    #end
  #end
  #if (!$hasArrays)
    return java.util.Objects.hash(##
      #set($i = 0)
      #if($superHasHashCode)
       super.hashCode() ##
       #set($i = 1)
      #end
      #foreach($field in $fields)
        #if ($i > 0)
        , ##
        #end
        $field.accessor ##
        #set($i = $i + 1)
      #end
    );
  #else
    #set($resultName = $helper.getUniqueLocalVarName("result", $fields, $settings))
    #set($resultAssigned = false)
    #set($resultDeclarationCompleted = false)
    int $resultName ##
    #if($hasNoArrays)
      = java.util.Objects.hash(##
        #set($i = 0)
        #if($superHasHashCode)
         super.hashCode() ##
         #set($i = 1)
        #end
        #foreach($field in $fields)
          #if(!$field.array)
            #if ($i > 0)
            , ##
            #end
            $field.accessor ##
            #set($i = $i + 1)
          #end
        #end
      );
      #set($resultAssigned = true)
      #set($resultDeclarationCompleted = true)
    #elseif($superHasHashCode)
       = super.hashCode(); ##
      #set($resultAssigned = true)
      #set($resultDeclarationCompleted = true)
    #end
    #foreach($field in $fields)
      #if($field.array)
        #if ($resultDeclarationCompleted)
          $resultName ##
        #end
        = ##
        #if ($resultAssigned)
          31 * $resultName + ##
        #end
        java.util.Arrays.hashCode($field.accessor);
        #set($resultAssigned = true)
        #set($resultDeclarationCompleted = true)
      #end
    #end  
  return $resultName;
  #end
#end
}
```

For `private String field`, it generates:

```java
@Override
public boolean equals(final Object o) {
    if (this == o) return true;
    if (!(o instanceof final ThatClass that)) return false;
    return field.equals(that.field);
}

@Override
public int hashCode() {
    return Objects.hash(field);
}
```

### Q String concat (+)

```java
public java.lang.String toString() {
#if ( $members.size() > 0 )
#set ( $i = 0 )
    return "$classname{" +
#foreach( $member in $members )
#if ( $i == 0 )
    "##
#else
    ", ##
#end
#if ( $member.objectArray )
#if ($java_version < 5)
$member.name=" + ($member.accessor == null ? null : java.util.Arrays.asList($member.accessor)) +
#else
$member.name=" + java.util.Arrays.toString($member.accessor) +
#end
#elseif ( $member.primitiveArray && $java_version >= 5)
$member.name=" + java.util.Arrays.toString($member.accessor) +
#elseif ( $member.string )
$member.name='" + $member.accessor + '\'' +
#else
$member.name=" + $member.accessor +
#end
#set ( $i = $i + 1 )
#end
    '}';
#else
    return "$classname{}";
#end
}
```

For `private String field`, it generates:

```java
@Override
public String toString() {
    return "ThatClass{" +
        "field='" + field + '\'' +
        '}';
}
```
