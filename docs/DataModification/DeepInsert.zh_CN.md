---
layout: default
title: Deep Insert
parent: 数据更改
nav_order: 2
---

# Deep Insert
当创建一条数据时，同时创建该条数据的子对象，这个就叫Deep Insert。
```xml
<Schema xmlns="http://docs.oasis-open.org/odata/ns/edm" Namespace="com.dpbird">
    <EntityType Name="ProductType">
        <Key>
            <PropertyRef Name="productTypeId"/>
        </Key>
        <Property Name="productTypeId" Type="Edm.String"/>
        <Property Name="description" Type="Edm.String"/>
    </EntityType>
    <EntityType Name="ProductPrice">
        <Key>
            <PropertyRef Name="productId"/>
            <PropertyRef Name="currencyUomId"/>
        </Key>
        <Property Name="productId" Type="Edm.String"/>
        <Property Name="currencyUomId" Type="Edm.String"/>
        <Property Name="price" Type="Edm.Double"/>
        <NavigationProperty Name="Product" Type="com.dpbird.Product">
            <ReferentialConstraint Property="productId" ReferencedProperty="productId"/>
        </NavigationProperty>
    </EntityType>
    <EntityType Name="ProductCategory">
        <Key>
            <PropertyRef Name="productCategoryId"/>
        </Key>
        <Property Name="productCategoryId" Type="Edm.String"/>
        <Property Name="productCategoryName" Type="Edm.String"/>
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
    <EntityContainer Name="DefaultContainer">
        <EntitySet Name="Products" EntityType="com.dpbird.Product"/>
    </EntityContainer>
</Schema>
```
创建一条Product数据的同时，创建两条其对应的ProductPrice
```html
POST serviceRoot/Products
OData-Version: 4.0
Content-Type: application/json;odata.metadata=minimal
Accept: application/json
```
请求的Payload是一段JSON。
```json
{
  "@odata.type" : "com.dpbird.Product",
  "productId": "10000",
  "productName" : "Dell XP15 32GB",
  "ProductPrice": [
    {
      "currencyUomId": "USD",
      "price": 2560.00
    },
    {
      "currencyUomId": "CNY",
      "price": 15000.00
    }
  ]
}
```
注意，子对象里的productId，可以不用写，因为ProductPrice里有ReferentialConstraint的定义。
