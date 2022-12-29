---
layout: default
title: EntityType新增修改删除
parent: 数据更改
nav_order: 1
---

# EntityType新增修改删除
OData中有一套方案用于修改EntityType，也就是新增、修改、删除
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
## 新增
新增一条EntityType，采用的是http的POST方式。
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
  "productName" : "Dell XP15",
  "productType" : "FinishedProduct"
}
```
注意，请求的地址是EntitySet，就像我们先前所描述的，任何EntityType是无法直接访问的，包括新建，必须通过EntitySet。这样就会在系统里创建一条数据。
## 修改
修改一条EntityType，采用的是http的PATCH方式
```html
PATCH serviceRoot/Products('10000')
OData-Version: 4.0
Content-Type: application/json;odata.metadata=minimal
Accept: application/json
```
请求的Payload也是一段JSON。
```json
{
  "@odata.type" : "com.dpbird.Product",
  "productName" : "Dell XP15 32GB"
}
```
同样，请求的地址是EntitySet，而不是EntityType。
## 删除
删除一条EntityType，采用的是http的DELETE方式
```html
DELETE serviceRoot/Products('10000')
```