# Action

Action与Function的主要区别是，Action可以看作是写操作，而Function是读操作。其它的区别还有：Action是post调用，而Function是get调用；Action可以没有返回，而Function必须有返回。

与Function一样，在metadata的定义中，定义的Action是无法直接调用的，这个概念跟EntityType的定义一样，EntityType也是无法直接访问的。访问Action有两种途径，一个是定义ActionImport，一个是将Action定义为bound Action。
ActionImport：
```xml
<Schema>
    <Action Name="TestAction">
        <Parameter Name="partyId" Type="Edm.String" Nullable="false"/>
        <Parameter Name="otherParam" Type="Edm.String" />
        <ReturnType Type="Edm.String"/>
    </Action>
    …
    <EntityContainer Name="DefaultContainer">
        <ActionImport Name="testImportAction" Action="com.dpbird.TestAction"/>
    </EntityContainer>
</Schema>
```

注意，这里定义的ActionImport，就是http访问的，真正的Action的定义，是无法直接访问，需要通过ActionImport。

Bound Action：
```xml
<Schema>
    <EntityType Name="Party">
        <Key>
            <PropertyRef Name="productId"/>
        </Key>
        <Property Name="partyId" Type="Edm.String" Nullable="false"/>
        …
        <Property Name="isEnable" Type="Edm.Boolean"/>
    </EntityType>
    …
    <Action Name="TestBoundAction" IsBound="true">
        <Parameter Name="party" Type="com.dpbird.Party" Nullable="false"/>
        <Parameter Name="otherParam" Type="Edm.String" />
        <ReturnType Type="Edm.String"/>
    </Action>
</Schema>
```
注意这里的IsBound是true。对于bound action，也不能直接访问Action，而是需要通过bound的对象Party进行访问

http://server.com/odata.svc/Party(‘10000’)/TestBoundAction

Action的访问，是Post方法，参数不是写在url中，而是在http post的body中
```json
{
  "otherParm":"9999"
}
```
注意，Action定义的第一个参数，就是bound的对象，所以，后续的Action调用，不要再传第一个参数了。