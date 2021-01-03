# Strapi 使用心得

[TOC]

#### - 檔案位置

> C:\Users\yfm\Documents\WebDev\react-learning\my-project

#### - 登入

> http://localhost:1337/admin
>
> 帳號：yaffmo@gamil.com
>
> 密碼：z3695123Z

#### - 設定流程

## 1. Install Strapi and Create a new project

yarn

npx

```bash
yarn create strapi-app my-project --quickstart
```

## 2. Create an Administrator user

Navigate to [**http://localhost:1337/admin** (opens new window)](http://localhost:1337/admin).

Complete the form to create the first **Administrator** user.

## 3. Create a Restaurant Content Type

Navigate to [**PLUGINS** - **Content Type Builder** (opens new window)](http://localhost:1337/admin/plugins/content-type-builder), in the left-hand menu.

- Click the **"+ Create new collection type"** link
- Enter `restaurant`, and click `Continue`
- Click the **"+ Add another Field"** button
- Click the **Text** field
- Type `name` in the **Name** field
- Click over to the **ADVANCED SETTINGS** tab, and check the `Required field` and the `Unique field`
- Click the **"+ Add another Field"** button
- Click the **Rich Text** field
- Type `description` under the **BASE SETTINGS** tab, in the **Name** field
- Click `Finish`
- Click the **Save** button and wait for Strapi to restart

## 4. Create a Category Content type

Navigate back to [**PLUGINS** - **Content Type Builder** (opens new window)](http://localhost:1337/admin/plugins/content-type-builder), in the left-hand menu.

- Click the **"+ Create new collection type"** link
- Enter `category`, and click `Continue`
- Click the **"+ Add another Field"** button
- Click the **Text** field
- Type `name` under the **BASE SETTINGS** tab, in the **Name** field
- Click over to the **ADVANCED SETTINGS** tab, and check the `Required field` and the `Unique field`
- Click the **"+ Add another field"** button
- Click the **Relation** field
- On the right side, click the **Category** dropdown and select, `Restaurant`
- In the center, select the icon that represents `many-to-many`. The text should read, `Categories has and belongs to many Restaurants`
- Click `Finish`
- Click the **Save** button and wait for Strapi to restart

## 5. Add content to "Restaurant" Content Type

Navigate to [**COLLECTION TYPES** - **Restaurants** (opens new window)](http://localhost:1337/admin/plugins/content-manager/restaurant?source=content-manager), in the left-hand menu.

- Click on **+ Add New Restaurant** button. Type `Biscotte Restaurant` in the **Restaurant** field. Type `Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers.` into the **Description** field.
- Click **Save**.

You will see your restaurant listed in the entries.

## 6. Add categories to the "Category" Content Type

Navigate to [**COLLECTION TYPES** - **Categories** (opens new window)](http://localhost:1337/admin/plugins/content-manager/category?source=content-manager).

- Click on **+ Add New Category** button. Type `French Food` in the **Category** field. Select `Biscotte Restaurant`, on the right from **Restaurant (0)**.
- Click **Save**.

You will see the **French Food** category listed in the entries.

- Click on **+ Add New Category** button. Type `Brunch` in the **Category** field. **DO NOT ADD IT HERE** to `Biscotte Restaurant`.
- Click **Save**.

You will see the **Brunch** category listed in the entries.

Navigate back to [**COLLECTION TYPES** - **Restaurants** (opens new window)](http://localhost:1337/admin/plugins/content-manager/restaurant?source=content-manager).

- Click on `Biscotte Restaurant`.
- On the right, under **Categories(1)**, `select` the `Add an item...`, and add **Brunch** as a category for this restaurant, and click the **Save** button.

You have now seen **two different ways** to use the **relation** field type to add and connect relations between Content Types.

## 7. Set Roles and Permissions

Navigate to [**SETTINGS** - **User's roles** (opens new window)](http://localhost:1337/admin/settings/users-permissions/roles).

- Click the **Public** Role.
- Scroll down under **Permissions**, open the **Application** tab and find **Restaurant**. Click the checkbox next to **find** and **findone**.
- Repeat and find **Category**. Click the checkbox next to **find** and **findone**.
- Click **Save**.

## 8. Publish the content

By default, any content you create is saved as a draft. To publish your content:

Navigate back to [**COLLECTION TYPES** - **Categories**(opens new window)](http://localhost:1337/admin/plugins/content-manager/collectionType/application::category.category)

- Click the **Draft** button on the **Brunch** category.
- Click **Publish** button.
- In the **Please confirm** dialog, click **Yes, publish** button.
- Repeat for the **French food** category and **Biscotte Restaurant**.

## 9. Consume the Content Type's API

Here we are! The list of **restaurants** is accessible at [`http://localhost:1337/restaurants` (opens new window)](http://localhost:1337/restaurants).



##### 