---
title: "Асинхронные запросы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Асинхронные запросы

Асинхронные запросы избежать, блокирующий поток, пока запрос выполняется в базе данных. Это может быть полезно предотвратить замораживание пользовательского интерфейса толстая клиентского приложения. Асинхронные операции можно также увеличить пропускную способность в веб-приложении, где поток может быть освобожден для обслуживания других запросов завершения операции базы данных. Дополнительные сведения см. в разделе [асинхронное программирование в C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Основные EF не поддерживает несколько параллельных операций, которые выполняются на том же экземпляре контекста. Всегда следует подождать завершения перед началом следующей операции операции. Обычно это делается с помощью `await` ключевое слово для каждой асинхронной операции.

Entity Framework Core предоставляет набор методов асинхронного расширения, которые можно использовать в качестве альтернативы методы LINQ, вызывающие выполнение запроса и результатов, возвращенных. Примеры включают `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`и т. д. Нет асинхронные версии операторы LINQ например `Where(...)`, `OrderBy(...)`и др., так как эти методы построения вверх по дереву выражения LINQ, а не вызывают запрос для выполнения в базе данных.

> [!IMPORTANT]  
> EF Core асинхронные методы расширения определяются в `Microsoft.EntityFrameworkCore` пространства имен. Это пространство имен должны импортироваться для методы, которые будут доступны.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
