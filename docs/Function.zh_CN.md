# Function

在OData的metadata中，有Function及Action的定义。Function和Action的主要区别在于，Function是只读的，客户端调用Function后，服务端不会有任何状态影响。而Action代表的是某种写操作，客户端调用后，服务端会产生状态影响（说直接点就是有数据更改了）。

Function的调用，是通过HTTP GET的方式调用，具体的调用：
http://server.com/odata.svc/testImportFunction(partyId=’10000’,otherParam=’9999’)

在metadata的定义中，定义的Function是无法直接调用的，这个概念跟EntityType的定义一样，EntityType也是无法直接访问的。访问Function有两种途径，一个是定义FunctionImport，一个是将Function定义为bound Function。
FunctionImport：
```xml
<Schema>
    <Function Name="TestFunction">
        <Parameter Name="partyId" Type="Edm.String" Nullable="false"/>
        <Parameter Name="otherParam" Type="Edm.String" />
        <ReturnType Type="Edm.String"/>
    </Function>
	…
    <EntityContainer Name="DefaultContainer">
        <FunctionImport Name="testImportFunction" Function="com.dpbird.TestFunction"/>
        …
    </EntityContainer>
</Schema>
```

注意，这里定义的FunctionImport，就是http访问的，真正的Function的定义，是无法直接访问，需要通过FunctionImport。

Bound Function：
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
    <Function Name="TestBoundFunction" IsBound="true">
        <Parameter Name="party" Type="com.dpbird.Party" Nullable="false"/>
        <Parameter Name="otherParam" Type="Edm.String" />
        <ReturnType Type="Edm.String"/>
    </Function>
</Schema>
```
注意这里的IsBound是true。对于bound function，也不能直接访问Function，而是需要通过bound的对象Party进行访问

http://server.com/odata.svc/Party(‘10000’)/TestBoundFunction(otherParam=’9999’)

注意，Function定义的第一个参数，就是bound的对象，所以，后续的Function调用，不要再传第一个参数了。


