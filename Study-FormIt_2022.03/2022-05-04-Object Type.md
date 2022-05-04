---
title: Object Type
date: 2022-05-04
author: ideook
layout: post
---


```js
// nTypeCode
// WSM.nObjectType.nUnSpecifiedType = 0
// WSM.nObjectType.nBodyType = 1
// WSM.nObjectType.nLumpType = 2
// WSM.nObjectType.nShellType = 3
// WSM.nObjectType.nFaceType = 4
// WSM.nObjectType.nLoopType = 5
// WSM.nObjectType.nCoedgeType = 6
// WSM.nObjectType.nEdgeType = 7
// WSM.nObjectType.nVertexType = 8

let aPIGetAllObjectsByTypeReadOnly = await WSM.APIGetAllObjectsByTypeReadOnly(0, {nTypeCode}); 
console.log("aPIGetAllObjectsByTypeReadOnly", aPIGetAllObjectsByTypeReadOnly);
```
