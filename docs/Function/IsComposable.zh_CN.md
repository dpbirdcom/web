---
layout: default
title: IsComposable
parent: Function
nav_order: 2
---

# Function的IsComposable

在通常情况下，进行多段式访问，类似http://server.com/odata.svc/aaa/bbb/ccc/ddd，Function必须放在最后一段（也就是ddd可以是Function）。而如果需要Function出现在前面，如ccc是Function，则Function必须申明为IsComposable。

```xml
<Schema>
    <EntityType Name="Party">
        <Key>
            <PropertyRef Name="partyId"/>
        </Key>
        <Property Name="partyId" Type="Edm.String" Nullable="false"/>
        …
        <Property Name="isEnable" Type="Edm.Boolean"/>
    </EntityType>
    <EntityType Name="Product">
        <Key>
            <PropertyRef Name="productId"/>
        </Key>
        <Property Name="productId" Type="Edm.String"/>
        <Property Name="productName" Type="Edm.String"/>
        <Property Name="productType" Type="com.dpbird.ProductType"/>
        <Property Name="primaryProductCategoryId" Type="Edm.String"/>
        <Property Name="productTags" Type="Collection(Edm.String)"/>
        <NavigationProperty Name="PrimaryProductCategory" Type="com.dpbird.ProductCategory">
            <ReferentialConstraint Property="primaryProductCategoryId" ReferencedProperty="productCategoryId"/>
        </NavigationProperty>
        <NavigationProperty Name="ProductPrice" Type="Collection(com.dpbird.ProductPrice)"/>
    </EntityType>
    <EntityType Name="ProductCategory">
        <Key>
            <PropertyRef Name="productCategoryId"/>
        </Key>
        <Property Name="productCategoryId" Type="Edm.String"/>
        <Property Name="productCategoryName" Type="Edm.String"/>
    </EntityType>
    …
    <Function Name="TestBoundFunction" IsBound="true" IsComposable="true">
        <Parameter Name="party" Type="com.dpbird.Party" Nullable="false"/>
        <Parameter Name="otherParam" Type="Edm.String" />
        <ReturnType Type="com.dpbird.Product"/>
    </Function>
</Schema>
```

http://server.com/odata.svc/Party(‘10000’)/TestBoundFunction(otherParam=’9999’)/PrimaryProductCategory

这条查询首先获取Party('10000')数据，传给TestBoundFunction，返回的结果是个Product，最后获取这个Product的PrimaryProductCategory数据。


