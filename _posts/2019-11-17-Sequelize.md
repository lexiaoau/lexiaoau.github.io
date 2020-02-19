---
author: lexiao
comments: true
date: 2019-11-17 11:11:11+00:00
layout: post
link: http://localhost/blog/?p=440
published: true
slug: null
title: Sequelize
wordpress_id: 440
tags: recent fe
categories:
- javascript
---

# Association

## 增加 Association

### 步骤 （例如 Session.belongsTo( models.Title )

    1. 在数据库里面， Session表里，增加列 “TitleId”，关联到 Titles 表的某一行。
    2. 在 Session的model 文件里面，增加   Agenda_Session.belongsTo( models.Title );
    3. 现在可以抓取数据了。参照以下代码。

```js


//// ------------------   find with association   ------------------------------
    res = await db.Agenda_Session.findAll({
        include: [db.Part, db.Title],
        where: {
            id: 2
        },
    });


//// ------------------   create with association   ------------------------------

        const ses = await db.Agenda_Session.create({
        next_layout_id: 32188,
        next_page_id: 32199,
    }) ;    

    const part = await db.Part.create({has_title_bar: false, title: 'mytitle-1234'}   );
    ses.setPart(part);              // magic function


//// ------------------   嵌套 get   ------------------------------

        res = await db.Person.findAll({ 
        include: [
            { model: db.Person_Title, include: [
                { model: db.Title,  }
            ] }
        ],
        where: {
            id: 2,
        }
    });

```


## 增加 one-to-many Association

### 步骤 （例如 Industry.hasMany(models.SubIndustry, { as: 'Subindustry'} ); )

- 创建model   Industry 和 SubIndustry
- 在 Industry 的 model  js 文件里面，添加   Industry.hasMany(models.SubIndustry, { as: 'Subindustry'} );
- 在 SubIndustry 的 model  js 文件里面，添加   SubIndustry.belongsTo(models.Industry,  );
- 在数据库中创建表  SubIndustries   ， 这个可以通过 migration 完成 。
- 在表 SubIndustries ，手工添加列   IndustryId

在代码中如下执行查询：

```js

        const dbRes = await db.Industry.findAll({
            include: [{model: db.SubIndustry , as : 'Subindustry'}],
        });
        const sub1 = dbRes[0].Subindustry ;
        
        //-----------------------------------------------------------------------
        const subind1 = await db.SubIndustry.create({desp: 'sub iij512' }   );

        const parentIndustry = await db.Industry.findOne({            where: {                id: 8,            }        })

        await subind1.setIndustry( parentIndustry ) ;           //*****   "set"  , not "add", "add" is for list not single one

```

## 增加 many-to-many Association

### 步骤 （例如 SubIndustry 和 Deal )

- 创建model   Deal 和 SubIndustry
- 添加  SubIndustry.belongsToMany(models.Deal, { through: 'DealSubindustry'} );
- 添加  Deal.belongsToMany(models.SubIndustry, { through: 'DealSubindustry'} );
- 在数据库中创建表  DealSubindustries   ， 这个可以通过 model 和 migration 完成 。
    - npx sequelize-cli model:generate --name DealSubindustry --attributes DealId:integer,SubindustryId:integer
- 在表 DealSubindustries ，添加mapping id

在代码中如下执行查询：

```js

        const dbRes = await db.Deal.findAll({
            include: [db.SubIndustry ],
            where : {
                id: 100
            }
        });
        console.log(dbRes[0].SubIndustries);

```




















