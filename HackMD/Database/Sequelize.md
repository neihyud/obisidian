---
title: Sequelize

---

# Sequelize
## Migration
- `QueryInterface`: set instruction, middle between you and database
- 'addColumn': add new Column
- `npx sequelize-cli db:migrate`: create migration from models


```javascript=
// add column
const queryInterface = sequelize.getQueryInterface();
return queryInterface.addColumn({
  tableName: 'SomeTableNameHere',
  schema: 'mySchema'
}, 'columnNameHere', {
  type: Sequelize.DATE,
  allowNull: true,
});
```

```java=
const queryInterface = sequelize.getQueryInterface();

  const tableDefinition = await queryInterface.describeTable({
    tableName: 'admin_transaction',
    schema: 'barcode00.myshopify.com',
  })
      
  if (!tableDefinition.last_payment_time) {
    return queryInterface.addColumn({
      tableName: 'admin_transaction',
      schema: 'barcode00.myshopify.com',
    }, 'last_payment_time', {
      type: DataTypes.STRING,
      allowNull: true,
    });
  }

      return false;
    });
```