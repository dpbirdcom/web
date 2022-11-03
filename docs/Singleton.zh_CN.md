# Singleton

在OData的metadata定义中，有Singleton的定义。我们知道EntitySet是EntityType请求的一种入口，还有一种入口就是Singleton。

Singleton与EntitySet最大不同，Singleton只能返回一条数据，不需要给主键，不需要给查询条件，直接就给出数据。Singleton如果做个类比，好比web应用中session里的对象，比如说当前用户。
```xml
<Schema>
    <EntityType Name="UserLogin">
        <Key>
            <PropertyRef Name="userLoginId"/>
        </Key>
        <Property Name="userLoginId" Type="Edm.String" Nullable="false"/>
        <Property Name="currentPassword" Type="Edm.String" Nullable="false"/>
        …
        <Property Name="disabledBy" Type="Edm.Boolean"/>
        <Property Name="partyId" Type="Edm.Boolean"/>
    </EntityType>
    …
    <EntityContainer Name="DefaultContainer">
        <Singleton Name="Me" EntityType="com.dpbird.UserLogin"/>
        …
    </EntityContainer>
</Schema>
```

这个例子中，作为Singleton的Me，可以直接访问，而返回的对象就是UserLogin，代表当前用户的UserLogin。
http://server.com/odata.svc/Me
注意，Me不是OData的关键词，Me的具体解释，是OData服务端的事情。