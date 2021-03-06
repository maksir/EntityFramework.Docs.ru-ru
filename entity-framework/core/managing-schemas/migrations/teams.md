---
title: "Миграция в средах групп - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Миграция в средах групп
===============================
При работе с миграции в средах рабочих групп, необходимо проявлять особое внимание в файле моментального снимка модели. Этот файл может сообщить, при миграции вашей помощником объединяет полностью с вашими из Если необходимо разрешить конфликт, повторное создание миграции до предоставления доступа к ней.

<a name="merging"></a>слияние
-------
При миграции объединить с другими членами команды, можно получить в файле моментального снимка модели конфликтов. Если оба изменения не связаны, слияния является тривиальным, и два миграции могут сосуществовать. Например могут получить конфликта слияния в конфигурации типа сущности customer, выглядит следующим образом:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Так как оба эти свойства должны существовать в конечной модели, Завершение слияния, добавив оба свойства. Во многих случаях системы управления версиями может автоматически объединить такие изменения автоматически.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

В этих случаях миграции и миграции вашей помощником независимо друг от друга. Поскольку любое из них может быть применена во-первых, не нужно вносить дополнительные изменения в миграции перед предоставлением совместного доступа с другими участниками команды.

<a name="resolving-conflicts"></a>Разрешение конфликтов
-------------------
Иногда обнаруживается конфликт true при слиянии снимка модели. Например вы и ваш партнер по группе каждого было изменено и то же свойство.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

При возникновении такого рода конфликт необходимо разрешить ее, повторно создав переноса. Выполните следующие действия.

1. Прервать слияния и отката в рабочий каталог перед слиянием
2. Удаление перехода (сохранением изменений модели)
3. Слияние изменений ваш находится в рабочий каталог
4. Снова добавьте миграции

После этого можно применять два миграции в правильном порядке. Их миграция применяется в первую очередь, переименование столбца *псевдоним*, после переноса переименовывает его *Username*.

Миграцию можно безопасно использовать с остальными участниками команды.
