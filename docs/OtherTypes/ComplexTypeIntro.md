---
layout: default
title: ComplexType简介
parent: 其它数据类型
nav_order: 1
last_modified_date: Feb 21 2018 at 10:02 AM
---

# ComplexType
如果需要用到一些复杂的数据类型，可以定义ComplexType。如果把EntityType看作是数据库的表，那ComplexType可以看作Java Bean。
```xml
<Schema>
    <ComplexType Name="TestObjectOne">
        <Property Name="testObjectOneId" Type="Edm.String" Nullable="false"/>
        <Property Name="amount" Type="Edm.Decimal" Nullable="false"/>
        <Property Name="testDate" Type="Edm.DateTimeOffset"/>
    </ComplexType>
    <Action Name="testImportActionComplex">
        <Parameter Name="partyId" Type="Edm.String"/>
        <Parameter Name="otherParam" Type="Edm.String"/>
        <ReturnType Type="com.dpbird.TestObjectOne"/>
    </Action>
    ...
</Schema>
```
POST http://server/odata.svc/testImportActionComplex
```json
{
  "partyId": "admin",
  "otherParam": "9999"
}
```
返回结果
```json
{
  "@odata.context":"$metadata#com.dpbird.TestObjectOne",
  "@odata.metadataEtag":"1663051319573",
  "testObjectOneId":"10030",
  "amount":10,
  "testDate":"2022-09-13T07:17:36.718Z"
}
```
以上只是一个简单的例子，实际项目中，EnumType和ComplexType，可以用在EntityType的Property，Function和Action的Parameter，Function和Action
的ReturnType，也可以用在ComplexType的Property中。
