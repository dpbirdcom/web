---
layout: default
title: EntitySet
nav_order: 5
---

# EntitySet

在OData的metadata定义中，有EntityType的定义，我们可以简单认为这就是定义了一张表。还有就是EntitySet的定义，许多人对于什么是EntitySet，以及应该怎样定义EntitySet感到困惑。

EntitySet是OData请求的真正入口。如果metadata只定义了EntityType，没有定义对应的EntitySet，我们是无法直接访问到这个EntityType的。
```xml
<Schema>            
    <EntityType Name="Product">
        <Key>
            <PropertyRef Name="productId"/>
        </Key>
        <Property Name="productId" Type="Edm.String" Nullable="false"/>
        <Property Name="productTypeId" Type="Edm.String" Nullable="false"/>
    …
        <Property Name="isVariant" Type="Edm.Boolean"/>
        <Property Name="isVirtual" Type="Edm.Boolean"/>
    </EntityType>
    …
    <EntityContainer Name="DefaultContainer">
        <EntitySet Name="Products" EntityType="com.dpbird.Product"/>
        …
    </EntityContainer>
</Schema>
```

这个例子中，我们无法直接访问http://server.com/odata.svc/Product，这是不允许的，必须通过EntitySet，http://server.com/odata.svc/Products
如果不带任何条件，理论上会返回所有的Product对象。但是，EntitySet的解释是交给服务端的，服务端是可以定义具体的EntitySet代表什么业务逻辑数据。
比如说，我们对于同一个EntityType，我们定义了多个EntitySet

```xml
<Schema>
    …
    <EntityContainer Name="DefaultContainer">
        <EntitySet Name="Products" EntityType="com.dpbird.Product"/>
        <EntitySet Name="VirtualProducts" EntityType="com.dpbird.Product"/>
        <EntitySet Name="VariantProducts" EntityType="com.dpbird.Product"/>
    </EntityContainer>
</Schema>
```

这三个EntitySet有什么不同，是由服务端来决定，Products可能返回所有的产品，VirtualProducts可能返回所有的虚拟产品，VariantProducts可能返回所有的变型产品。

同样，我们如果访问单条数据，也必须从EntitySet入口。比如要访问id为10000的Product，我们不能http://server.com/odata.svc/Product(‘10000’)，必须走EntitySet，应该是http://server.com/odata.svc/Products(‘10000’)。
如果我们访问http://server.com/odata.svc/VirtualProducts(‘1000’)
如果10000是个虚拟产品，那这个请求就返回10000的产品，如果这个产品不是虚拟产品，那这个请求就需要返回http code 404，代表不存在。

其实，除了EntitySet，数据访问还有一个入口，是Singleton，这个留在今后再描述。