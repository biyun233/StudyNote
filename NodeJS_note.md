# Basic

### Create a server

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    console.log(req.url, req.method, req.headers);
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><h1>Hello from my node.js Server!</h1></body>');
    res.write('</html>');
    res.end();
});

server.listen(3000); // port 
```

### Routing & Redirecting Requests

```javascript
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  //routing
  if (url === '/') {
    res.write('<html>');
    res.write('<head><title>My First Page</title></head>');
    res.write('<body><form action="/message" method="POST"><input type="text" name="message"><button type="submit">Send</button></form></body>');
    res.write('</html>');
    return res.end();
  }
  // redirecting
  if(url === '/message' && method === 'POST') {
    fs.writeFileSync('message.txt', 'DUMMY');
    res.statusCode = 302;
    res.setHeader('Location', '/');
    return res.end();
  }
  res.setHeader('Content-Type', 'text/html');
  res.write('<html>');
  res.write('<head><title>My First Page</title></head>');
  res.write('<body><h1>Hello from my node.js Server!</h1></body>');
  res.write('</html>');
  res.end();
});
server.listen(3000); // port

```

### Parse Request body

```javascript
const body = [];
req.on('data', (chunk) => {
  body.push(chunk); // chunk: <Buffer 6d 65 73 73 61 67 65 3d 73 71 6c 6d 74>
});
req.on('end', () => {
  const parsedBody = Buffer.concat(body).toString(); //parsedBody: message = qjdklt
  const message = parsedBody.split('=')[1]; // message: qjdklt
  fs.writeFileSync('message.txt', message);
});
res.statusCode = 302;
res.setHeader('Location', '/');
return res.end();
```

​		we can not do anything with chunk.

### Blocking code

​		`fs.writeFileSync` is a special method which will actually block code execution until this file is created, so there is another method `fs.writeFile`

```javascript
fs.writeFile('message.txt', message, err => {
        res.statusCode = 302;
        res.setHeader('Location', '/');
        return res.end();
      });
```

### Module System

##### 			app.js

```javascript
const http = require('http');
const routes = require('./routes');

const server = http.createServer(routes);
server.listen(3000); // port
```

##### 		routes.js

![nodejs1](img/nodejs1.jpeg)

# Improved

### NPM script

- ​	`npm init`

- ​	package.json

  ![nodejs2](img/nodejs2.jpeg)

- ​	`npm start`

-    3rd party Packages: no longer need to quit and restart the server to update

  ​	`npm install nodemon --save-dev`

  ​	`dev` indicates it is used only during the development,

  ​	without `dev` means it is production dependency.

  ![nodejs3](img/nodejs3.jpeg)

### Error Types

- ​	Syntax Errors
- ​    Runtime Errors
- ​    Logical Errors



# Express.js

### Install

`npm install --save express`

```javascript
const http = require('http');
const express = require('express');

const app = express();

const server = http.createServer(app);
server.listen(3000); // port
```

### Middleware

```javascript
const express = require('express');

const app = express();

app.use((req, res, next) => {
    console.log('In the middleware!');
    next();
});

app.use((req, res, next) => {
    console.log('In another middleware!');
  	res.send('<h1>Hello from server!</h1>')
});
app.listen(3000); // port
```

​		if we put `res.send` instead of `next()`, the seconde middleware will never be called.	

### Routes

```javascript
app.use('/add-product',(req, res, next) => {
    console.log('In another middleware!');
    res.send('<h1>The "Add Product" Page</h1>')
});
```

### Parsing Incoming Request

​		x-www-form-urlencoded <form>

![nodejs4](img/nodejs4.jpeg)		

​	要获取request里特定的value： req.body.title， 那么input的name也要为title

### Router

##### app.js

![nodejs5](img/nodejs5.jpeg)

##### 		admin.js

![nodejs6](img/nodejs6.jpeg)shop.js

![nodejs7](img/nodejs7.jpeg)

### Error Page

```javascript
app.use((req, res, next) => {
    res.status(404).send('<h1>Page not found!</h1>')
});
```

### Serving HTML Pages

- ​	origin navigation

![nodejs8](img/nodejs8.jpeg)

![nodejs9](img/nodejs9.jpeg)

- ​	helper function for navigation

  ```javascript
  const path = require('path')
  
  module.exports = path.dirname(require.main.filename)
  ```
  
  ```javascript
  const rootDir = require('../utils/path')
  
  router.get('/',(req, res, next) => {
      res.sendFile(path.join(rootDir, 'views', 'shop.html'));
  });
  ```

### Serving Files Statically

​		app.js

```javascript
app.use(express.static(path.join(__dirname, 'public')))
```

​		index.html

```html
<script src="/main.js"></script> <!--don't need /public -->
```

# Dynamic Content

### Templating Engine

![nodejs10](img/nodejs10.jpeg)

​		`npm install --save ejs pug express-handlebars`

```javascript
app.set('view engine', 'pug'); 
app.set('views', 'views'); //html 页面所在文件夹
```

### Outputting Dynamic Content

```javascript
router.get('/add-product',(req, res, next) => {
    res.render('add-product', {pageTitle: 'Add Product'})
});
```

### Pug example ( add-product.pug )

![nodejs11](img/nodejs11.jpeg)

#### Adding Layout

![nodejs12](img/nodejs12.jpeg)

#### extends layout

![nodejs13](img/nodejs13.jpeg)

### Ejs example

![nodejs14](img/nodejs14.jpeg)

#### Layout with partials

![nodejs15](img/nodejs15.jpeg)

# MVC

![nodejs16](img/nodejs16.jpeg)

### Models

```javascript
const products = []
module.exports = class Product {
    constructor(title) {
        this.title = title
    }

    save() {
        products.push(this)
    }

    static fetchAll() {
        return products
    }
}
```

### Controllers

```javascript
const Product = require('../models/product')
exports.getAddProduct = (req, res, next) => {
  res.render('add-product', { 
      pageTitle: 'Add Product', 
      path: '/add-product' 
    });
};

exports.postAddProduct = (req, res) => {
    const product = new Product(req.body.title)
    product.save()
    res.redirect('/')
};

exports.getProducts = (req, res, next) => {
    const products = Product.fetchAll()
    res.render('shop', {
        prods: products, 
        pageTitle: 'Shop', 
        path: '/'})
};
```

### Storing / Fetching Data with Files

```javascript
const fs = require('fs')
const path = require('path')
const rootDir = require('../utils/path')
const p = path.join(rootDir, 'data', 'products.json');

const getProductFromFile = (callback) => {
    fs.readFile(p, (err, fileContent) => {
        if(err) {
            callback([])
        } else {
            callback(JSON.parse(fileContent))
        }
    });
}
module.exports = class Product {
    constructor(title) {
        this.title = title
    }

    save() {
        getProductFromFile(products => {
            products.push(this)
            fs.writeFile(p, JSON.stringify(products), (err) => {
                console.log(err);
            })
        })
    }
    static fetchAll(callback) {
        getProductFromFile(callback)
    }
  // Product.fetchAll(products => {
  //     res.render('shop', {
  //        prods: products, 
  //        pageTitle: 'Shop', 
  //        path: '/'})
  //     })
}
```

# Dynamic Routes

### Extract Dynamic Params

```javascript
router.get('/products/:productId', shopController.getProduct);
```

```javascript
exports.getProduct = (req, res, next) => {
  const prodId = req.params.productId
  console.log(prodId)
  res.redirect('/')
};
```

### Passing Data with Include

​		when we use block with `include` ,  the params won't be passed automatically

```ejs
<%- include('../includes/add-to-cart.ejs', {product: product}) %>
```

# MYSQL

### Connecting App to the SQL

`npm install --save mysql2`

/utils/database.js

```javascript
const mysql = require('mysql2');

const pool = mysql.createPool({
    host: 'localhost',
    user: 'root',
    database: 'node',
    password: 'nodecomplete'
});

module.exports = pool.promise();
```

app.js

```javascript
const db = require('./util/database');

db.execute('SELECT * FROM products');
```

### Fetching Data

/models/product.js

```javascript
static fetchAll() {
    return db.execute('SELECT * FROM products')
  }
```

/controllers/shop.js

```javascript
  Product.fetchAll()
    .then(([rows, fieldData]) => {
      res.render('shop/index', {
        prods: rows, //products
        pageTitle: 'Shop',
        path: '/'
      });
  }).catch(err => {
    console.log(err);
  });
```

### Inserting Data

/models/product.js

```javascript
save() {
    return db.execute(
      'INSERT INTO products (title, price, description, 						imageUrl) VALUES (?, ?, ?, ?)',
      [this.title, this.price, this.description, 										this.imageUrl]
    );
  }
```

/controllers/admin.js

```javascript
product
  .save()
  .then(() => {
    res.redirect('/');
  })
  .catch(err => {
    console.log(err);
  });
```

### Fetching a specific Product with the "where" Condition

/models/product.js

```javascript
static findById(id) {
    return db.execute('SELECT * FROM products WHERE products.id = ?', [id]);
  }
```

/controllers.shop.js

```javascript
Product.findById(prodId)
  .then(([product]) => {
    res.render('shop/product-detail', {
      product: product[0], // product is an array
      pageTitle: product.title,
      path: '/products/' + prodId,
    });
  }).catch(err => {
    console.log(err);
  });
```

# Sequelize

### Definition

An Object-Relational Mapping Library

![nodejs17](img/nodejs17.jpeg)

### Core Concepts

![nodejs18](img/nodejs18.jpeg)

### Connecting to the Database

​		`npm install --save sequelize`

​		/utils/database.js

```javascript
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize('node', 'root', 'nodecomplete', { // database, username, password
  dialect: 'mysql',
  host: 'localhost',
});
module.exports = sequelize;
```

### Defining a Model

```javascript
const Sequelize = require('sequelize');
const sequelize = require('../util/database');
const Product = sequelize.define('product', {
  id: {
    type: Sequelize.INTEGER,
    autoIncrement: true,
    allowNull: false,
    primaryKey: true
  },
  title: Sequelize.STRING,
  price: {
    type: Sequelize.DOUBLE,
    allowNull: false
  },
  imageUrl: {
    type: Sequelize.STRING,
    allowNull: false
  },
  description: {
    type: Sequelize.STRING,
    allowNull: false
  }
});

module.exports = Product;
```

### Syncing JS Definitions to the Database

app.js

```javascript
const sequelize = require('./util/database');
sequelize.sync().then(result => {
    //console.log(result);
    app.listen(3000);
}).catch(err => {
    console.log(err);
});
```

This sync method syncs all models to the database by creating the appropriate tables.

### Inserting Data

```javascript
Product.create({
    title: title,
    price: price,
    imageUrl: imageUrl,
    description: description
  })
  .then(result => {
    console.log('Created Product');
  })
  .catch(err => {
    console.log(err);
  });
```

### Retrieving Data

```javascript
Product.findAll()
    .then(products => {
      res.render('shop/index', {
        prods: products,
        pageTitle: 'Shop',
        path: '/',
      });
    })
    .catch((err) => {
      console.log(err);
    });
```

### Getting a specific Product

```javascript
Product.findByPk(prodId) // 相当于findById
    .then(product => {
      res.render('shop/product-detail', {
        product: product,
        pageTitle: product.title,
        path: '/products/' + prodId,
      });
    })
    .catch((err) => {
      console.log(err);
    });
```

​		or

```javascript
Product.findAll({
    where: {
      id: prodId
    }
  }).then(products => {
    res.render('shop/product-detail', {
      product: products[0],
      pageTitle: product.title,
      path: '/products/' + prodId,
    });
  })
  .catch((err) => {
    console.log(err);
  });
```

### Updating Data

```javascript
Product.findByPk(prodId).then(product => {
    product.title = title;
    product.imageUrl = imageUrl;
    product.price = price;
    product.description = description;
    return product.save();
  })
  .then(result => {
    console.log('Updated Product!');
  	res.redirect('/admin/products');
  })
  .catch(err => {
    console.log(err);
  });
```

### Deleting Data

```javascript
Product.findByPk(prodId)
    .then(product => {
      return product.destroy();
    })
    .then(result => {
      console.log('DESTROYED Product!');
      res.redirect('/admin/products');
    })
    .catch((err) => {
      console.log(err);
    });
```

 ### One-To-Many Relationship

![nodejs19](img/nodejs19.jpeg)

app.js

```javascript
Product.belongsTo(User, {
  constraints: true,
  onDelete: 'CASCADE', // if a user is deleted, the product associated will also be deleted
});
User.hasMany(Product);
```

#### Association Method

causing Product belongs to User, or User has many product, we could use association method when we create product.

app.js

首先为每个incoming request 定义一个user，相当于登录用户

```javascript
app.use((req, res, next) => {
    User.findByPk(1)
    .then(user => {
        req.user = user;
        next();
    })
    .catch(err => {
        console.log(err);
    });
});
```

/controllers/admin.js

- ​	`createProduct`

```javascript
req.user.createProduct({
    title: title,
    price: price,
    imageUrl: imageUrl,
    description: description,
  })
    .then((result) => {
      console.log('Created Product');
      res.redirect('/admin/products');
    })
    .catch((err) => {
      console.log(err);
    });
```

- ​	`getProducts`

```javascript
req.user.getProducts({
    where: {
      id: prodId
    }
  })
    .then(products => {
      res.render('admin/edit-product', {
        pageTitle: 'Edit Product',
        path: '/admin/edit-product',
        editing: editMode,
        product: products[0],
      });
    })
    .catch((err) => {
      console.log(err);
    });
```

### Many-To-Many Relationship

```javascript
Cart.belongsToMany(Product, { through: CartItem });
```

​		ex: An Author can write several Books, and a Book can be written by several Authors

#### Association Method

- ​	`getProducts`

```javascript
cart.getProducts()
  .then((products) => {
  res.render('shop/cart', {
    products: products,
    path: '/cart',
    pageTitle: 'Your Cart',
    });
  })
  .catch((err) => {
  console.log(err);
  });
```

![nodejs20](img/nodejs20.jpeg)

每一个 product 相当于 cart 中的 cartItem ， 通过 p.cartItem 获取参数		

- ​	`addProduct`

```javascript
fetchedCart.addProduct(product, {
	through: {
		quantity: newQuantity
	}
});
```

- ​	`destroy`

```
product.cartItem.destroy();
```

### One-To-Many & Many-To-Many Relationships

```javascript
Order.belongsTo(User);
User.hasMany(Order);
Order.belongsToMany(Product, { through: OrderItem });
```

#### Association Method

- `get`

```javascript
req.user
    .getOrders({include: ['products']})
    .then(orders => {
      res.render('shop/orders', {
        path: '/orders',
        pageTitle: 'Your Orders',
        orders: orders
      });
    })
    .catch((err) => {
      console.log(err);
    });
```

​		`include`: 当 user 获取 orders 的时候，顺便获取 orders 中的 products

![nodejs21](img/nodejs21.jpeg)



# MongoDB

### Server

​		MongoDB Atlas

![nodejs22](img/nodejs22.jpeg)

![nodejs23](img/nodejs23.jpeg)	account: syviass@heymenology.com

### MongoDB Compass

![nodejs24](img/nodejs24.jpeg)

pwd: RVx1COV1vf3RxT8Z

### Connecting to the Database

`npm install --save mongodb`

/util/database.js

```javascript
const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;

let _db; // the underscore is to signal that thiswill only be used internally in this file
const mongoConnect = (callback) => {
  MongoClient.connect(
 'mongodb+srv://biyun:RVx1COV1vf3RxT8Z@cluster0.cxpte.mongodb.net/shop?retryWrites=true&w=majority', { useNewUrlParser: true, useUnifiedTopology: true }
  )
    .then((client) => {
      console.log('Connected!');
      _db = client.db(); // store a connection to database in the _db variable
      callback();
    })
    .catch((err) => {
      console.log(err);
      throw err;
    });
};

const getDb = () => {
  if(_db) {
    return _db;
  }
  throw 'No database found!';
}

exports.mongoConnect = mongoConnect;
exports.getDb = getDb;

```

app.js

```javascript
const mongoConnect = require('./util/database').mongoConnect;
mongoConnect(() => {
  app.listen(3000);
})
```

### Defining A Model

​		/models/product.js

```javascript
const getDb = require('../util/database').getDb;
const mongodb = require('mongodb');

class Product {
  constructor(title, price, description, imageUrl, id) {
    this.title = title;
    this.price = price;
    this.description = description;
    this.imageUrl = imageUrl;
    this._id = id ? new mongodb.ObjectId(id) : null;
  }
}


module.exports = Product;
```

​		/controllers/admin.js

```javascript
exports.postAddProduct = (req, res, next) => {
  const title = req.body.title;
  const imageUrl = req.body.imageUrl;
  const price = req.body.price;
  const description = req.body.description;
  const product = new Product(title, price, description, imageUrl);
  product
    .save()
    .then(result => {
      console.log('Created Product');
      res.redirect('/admin/products');
    })
    .catch(err => {
      console.log(err);
    });
};
```

### Inserting & Updating Data

```javascript
save() {
    const db = getDb();
    let dbOp;
    if(this._id) {
      // update
      dbOp = db.collection('products').updateOne({ _id: this._id }, // in Compass:
     // _id: ObjectId("60aeffa56849f7de1eb23156")
      { $set: this }
      );
    } else {
      dbOp = db.collection('products')
      .insertOne(this);
    }
    
    return dbOp
      .then((result) => {
        console.log(result);
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

### Fetching All data

​		/models/product.js

```javascript
  static fetchAll() {
    const db = getDb();
    return db
      .collection('products')
      .find()
      .toArray()
      .then(products => {
        return products;
      })
      .catch((err) => {
        console.log(err);
      }); // toArray: get rid of having all documents at once
  }
```

### Fetching Single Data

```javascript
static findById(prodId) {
    const db = getDb();
    return db
      .collection('products')
      .find({ _id: new mongodb.ObjectId(prodId) }) 
      .next() //the server don't know we will only get one document, so next() will return the next and in this case also the last document
      .then(product => {
        console.log(product);
        return product;
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

### Deleting Data

```javascript
 static deleteById(prodId) {
    const db = getDb();
    return db.collection('products')
      .deleteOne({ _id: new mongodb.ObjectId(prodId) })
      .then(result => {
        console.log('Deleted!');
      })
      .catch((err) => {
        console.log(err);
      });
  }
```

### Find Specific Data

- `find({ _id: { $in: productIds } })`

SELECT * FROM products WHERE product.id  `in`  productIds

- `find({ 'user._id': this._id })`

  ![nodejs25](img/nodejs25.jpeg)

  we use single quote when the field is nested

### Query for an Array Element that Meets Multiple Criteria

`db.inventory.find( { dim_cm: { $elemMatch: { $gt: 22, $lt: 30 } }})`

# Mongoose

### Definition

An Object-Relational Mapping Library

![nodejs26](img/nodejs26.jpeg)

### Core Concepts

![nodejs27](img/nodejs27.jpeg)

### Connecting to the Database

`npm install --save mongoose`

app.js

```javascript
const mongoose = require('mongoose');
mongoose
  .connect(
'mongodb+srv://biyun:RVx1COV1vf3RxT8Z@cluster0.cxpte.mongodb.net/shop?retryWrites=true&w=majority', { useNewUrlParser: true, useUnifiedTopology: true }
  )
  .then(result => {
    app.listen(3000);
  })
  .catch(err => {
    console.log(err);
  });
```

### Creating The Schema

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
const productSchema = new Schema({
    title: {
        type: String,
        required: true
    },
    price: {
        type: Number,
        required: true
    },
    description: {
        type: String,
        required: true
    },
    imageUrl: {
        type: String,
        required: true
    }

})

module.exports = mongoose.model('Product', productSchema);
```

### Saving Data

```javascript
 const product = new Product({
    title: title,
    price: price,
    imageUrl: imageUrl,
    description: description
  });
  product
    .save() // method from mongoose
    .then(result => {
      console.log('Created Product');
      res.redirect('/admin/products');
    })
    .catch(err => {
      console.log(err);
    });
```

### Fetching All Data

```javascript
Product.find()
    .then(products => {
      res.render('admin/products', {
        prods: products,
        pageTitle: 'Admin Products',
        path: '/admin/products'
      });
    })
    .catch(err => console.log(err));
```

### Fetching Single Data

```javascript
const prodId = req.params.productId;
Product.findById(prodId)
  .then(product => {
  res.render('shop/product-detail', {
    product: product,
    pageTitle: product.title,
    path: '/products'
  });
})
  .catch(err => console.log(err));
```

### Updating Data

```javascript
  Product.findById(prodId).then(product => {
    product.title = updatedTitle;
    product.description = updatedDesc;
    product.imageUrl = updatedImageUrl;
    product.price = updatedPrice;
    return product.save();
  })
    .then(result => {
      console.log('UPDATED PRODUCT!');
      res.redirect('/admin/products');
    })
    .catch(err => console.log(err));
```

### Deleting Data

```javascript
  const prodId = req.body.productId;
  Product.findByIdAndRemove(prodId)
    .then(() => {
      console.log('DESTROYED PRODUCT');
      res.redirect('/admin/products');
    })
    .catch(err => console.log(err));
```

### Using Relations with ref

​		/models/user.js

![nodejs28](img/nodejs28.jpeg)

​		/models/product.js

![nodejs29](img/nodejs29.jpeg)

```javascript
const product = new Product({
    title: title,
    price: price,
    imageUrl: imageUrl,
    description: description,
    userId: req.user // mongoose return the ID automatically
  });
```

### Fetching Relations

```javascript
exports.getProducts = (req, res, next) => {
  Product.find()
  	.select('title price -_id') // mongoose will just return title, price without _id
    .populate('userId') // populate allows fetching related user data, we could select the fields we want : populate('userId', 'name')
    .then(products => {
      console.log(products);
      res.render('admin/products', {
        prods: products,
        pageTitle: 'Admin Products',
        path: '/admin/products'
      });
    })
    .catch(err => console.log(err));
};

// Call the `populate()` method on a document to populate a path.
// Need to call `execPopulate()` to actually execute the `populate()`.
req.user
    .populate('cart.items.productId')
    .execPopulate()
```

without `populate`

![nodejs30](img/nodejs30.jpeg)

with `populate('creator')`

![nodejs31](img/nodejs31.jpeg)

### Adding Methods To Schema

```javascript
userSchema.methods.addToCart = function(product) {
    const cartProductIndex = this.cart.items.findIndex((cp) => {
        return cp.productId.toString() === product._id.toString();
      });
      let newQuantity = 1;
      const updatedCartItems = [...this.cart.items];
      if (cartProductIndex >= 0) {
        newQuantity = this.cart.items[cartProductIndex].quantity + 1;
        updatedCartItems[cartProductIndex].quantity = newQuantity;
      } else {
        updatedCartItems.push({
          productId: product._id,
          quantity: newQuantity,
        });
      }
      const updatedCart = { items: updatedCartItems };
      this.cart = updatedCart;
      return this.save();
}
```

### Return A Document with An Id

```javascript
 product: {...i.productId._doc}
```

### Find With Condition

```
Order.find({"user.userId": req.user._id})
```

# Session & Cookie

![nodejs32](img/nodejs32.jpeg)

### Cookie Example

![nodejs33](img/nodejs33.jpeg)

But cookie can be changed in the browser(client side), so we need session for help.

![nodejs34](img/nodejs34.jpeg)

`npm install --save express-session`

### Initialize the session

```javascript
const session = require('express-session');
app.use(
  session({ secret: 'my secret', resave: false, saveUninitialized: false })
);
```

### Example

![nodejs35](img/nodejs35.jpeg)

### Using MongoDB to Store Sessions

`npm install --save connect-mongodb-session`

app.js

```javascript
const session = require('express-session');
const MongoDBStore = require('connect-mongodb-session')(session);

const MONGODB_URI =
'mongodb+srv://biyun:RVx1COV1vf3RxT8Z@cluster0.cxpte.mongodb.net/shop?retryWrites=true&w=majority';
const app = express();
const store = new MongoDBStore({
  uri: MONGODB_URI,
  collection: 'sessions',
});

app.use(
  session({
    secret: 'my secret',
    resave: false,
    saveUninitialized: false,
    store: store,
  })
);

app.use((req, res, next) => {
  if(!req.session.user) {
    return next();
  }
  User.findById(req.session.user._id)
    .then((user) => {
      req.user = user; // we can manipulate with this user.
      next();
    })
    .catch((err) => console.log(err));
});
```

/controllers.js/auth.js

```javascript
exports.getLogin = (req, res, next) => {
  res.render('auth/login', {
    path: '/login',
    pageTitle: 'Login',
    isAuthenticated: false,
  });
};

exports.postLogin = (req, res, next) => {
  User.findById('60b04675bda0c00c459c8b13')
    .then((user) => {
      req.session.isLoggedIn = true;
      req.session.user = user; // req.session.user is juste data, we can not manipulate with it.
      req.session.save(err => { 
        if(err) {
          console.log(err);
        }
        res.redirect('/');
      })
    })
    .catch((err) => console.log(err));
};
```



### Deleting A Cookie

```javascript
exports.postLogout = (req, res, next) => {
  req.session.destroy(err => {
    console.log(err);
    res.redirect('/');
  });
};
```

# Authentication

![nodejs36](img/nodejs36.jpeg)

### Encrypt Password

`npm install --save bcryptjs`

```javascript

exports.postSignup = (req, res, next) => {
  const email = req.body.email;
  const password = req.body.password;
  const confirmPassword = req.body.confirmPassword;
  User.findOne({ email: email })
    .then(userDoc => {
      if(userDoc) {
        return res.redirect('/signup');
      }
      return bcrypt.hash(password, 12) // 12: rounds of hash applied
      .then(hashedPassword => {
        const user = new User({
          email: email,
          password: hashedPassword,
          cart: {
            items: []
          }
        });
        return user.save();
      }); 
    })
    .then(result => {
      res.redirect('/login');
    })
    .catch((err) => {
      console.log(err);
    });
};
```

### Login Compare Password

```javascript
bcrypt
  .compare(password, user.password)
  .then((doMatch) => {
  if (doMatch) {
    req.session.isLoggedIn = true;
    req.session.user = user;
    return req.session.save((err) => {
      console.log(err);
      res.redirect('/');
    });
  }
  res.redirect('/login');
})
```

### Using Middleware to Protect routes

​		/middleware/is-auth.js

```javascript
module.exports = (req, res, next) => {
    if(!req.session.isLoggedIn) {
        return res.redirect('/login');
    }
    next();
}
```

​		/routes/admin.js

```javascript
const isAuth = require('../middleware/is-auth');

const router = express.Router();

// /admin/add-product => GET
router.get('/add-product', isAuth, adminController.getAddProduct);
```

### CSRF Attacks

![nodejs37](img/nodejs37.jpeg)

​	The fake sites might send a request to your backend and they could theoretically use your session. So if we give every request a token which is a random hashed value, they can't guess it and they also can't steal it because a new token is generated for every page rendered.

### Using CSRF Token

`npm install --save csurf`

​	app.js

```javascript
app.use((req, res, next) => {
  res.locals.isAuthenticated = req.session.isAuthenticated;
  res.locals.csrfToken = req.csrfToken(); // This method is provided by the csrf middleware
  next();
});
```

after defining user, before using routes, we add CSRF Protection to all page we render.

​		In all forms, we define a token

```html
<form action="/create-order" method="POST">
	<input type="hidden" name="_csrf" value="<%= csrfToken %>">
	<button type="submit" class="btn">Order Now!</button>
</form>
```

​		The name `_csrf` is asked by the csrf middleware.

### Feedback -- flash error message

`npm install --save connect-flash`

app.js

```javascript
const flash = require('connect-flash');
app.use(flash());
```

​		/controllers/auth.js

​		postLogin

```javascript
if (!user) {
  req.flash('error', 'Invalid email or password.')
  return res.redirect('/login');
}
```

​	getLogin

```javascript
let message = req.flash('error'); // [] if empty
if(message.length > 0) {
  message = message[0];
} else {
  message = null;
}
res.render('auth/login', {
  path: '/login',
  pageTitle: 'Login',
  errorMessage: message
});
```

# Validation

`npm install --save express-validator`

### Basic

![nodejs38](img/nodejs38.jpeg)

​		the `check` function needs a field name which is defined in input.

​		/routes/auth.js

```javascript
const { check } = require('express-validator');
router.post('/signup', check('email').isEmail().withMessage('Please enter a valid email.'), authController.postSignup);

```

​		/controllers/auth.js

```javascript
const { validationResult } = require('express-validator');
// postSignup
const errors = validationResult(req);
  if(!errors.isEmpty()){
    console.log(errors.array());
    /*
    [
      {
        value: 'test',
        msg: 'Invalid value',
        param: 'email',
        location: 'body'
      }
    ]
    */
    return res.status(422).render('auth/signup', {
      path: '/signup',
      pageTitle: 'Signup',
      errorMessage: errors.array()[0].msg,
    });
  }
```

### More Validation

We can also use a different function besides `check` like `body`, `param` , `query` , `cookie ` , `header` .

```javascript
body(
      'password',
      'Please enter a password with only numbers and text and at least 5 characters.'
    )
      .isLength({ min: 5 })
      .isAlphanumeric(),
```

​		If we want the same error message (not the default message) for all validator, we can just add it as a second argument to the `body` .

### Custom Validation (equality)

```javascript
body('confirmPassword').custom((value, {req}) => {
        if(value !== req.body.password) {
            throw new Error('passwords have to match!')
        }
        return true;
    })
```

### Async Validation

```javascript
check('email')
      .isEmail()
      .withMessage('Please enter a valid email.')
      .custom((value, {req}) => {
        return User.findOne({ email: value }).then(userDoc => {
            if(userDoc) {
                return Promise.reject('E-mail exists already, please pick a different one.');
            }
        });
    }),
```

If we return a `Promise` , `express-validator` will wait for this promise to be fulfilled(实现), if it fulfills with nothing (no error), it treats this validation as successful. If it resolves with some rejection even though it is in a if block, express-validator will detect this rejection and store this as an error.

### Sanitizing Data

to ensure the data is stored in a uniform format.

email: `normalizeEmail()`

password: `trim()`

![nodejs39](img/nodejs39.jpeg)

# Error

![nodejs40](img/nodejs40.jpeg)

### Error Handling Middleware

```javascript
app.use((error, req, res, next) => {
  res.status(500).render('500', {
    pageTitle: 'Error!',
    path: '/500',
  });
})
```

```javascript
.catch((err) => {
      const error = new Error(err);
      error.httpStatusCode = 500;
      return next(error);
    });
```

# File Upload & Download

`npm install --save multer`

This package parses incoming requests for files.

### Simple Use

​		/views/edit-product.ejs

![nodejs41](img/nodejs41.jpeg)

​		app.js

```javascript
const multer = require('multer');
app.use(multer({dest: 'images'}).single('image')); 
// single defines only one file we will get
// {dest: 'images'} put the uploaded images in the folder images
//'image' is the name of input

```

​		/controllers/admin.js

![nodejs42](img/nodejs42.jpeg)

### More Configuration

​	app.js

#### 		Adjust Filename & Filepath

```javascript
const fileStorage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'images'); // null: error message, 'images': the place to store images
  },
  filename: (req, file, cb) => {
    cb(null, new Date().toISOString() + '-' + file.originalname)
  }
});

app.use(multer({storage: fileStorage}).single('image'));
```

#### 		Filtering Files by Mimetype

```javascript
const fileFilter = (req, file, cb) => {
  if(file.mimetype === 'image/png' || 
     file.mimetype === 'image/jpg' || 
     file.mimetype === 'image/jpeg') {
    cb(null, true); // true: we want to store this file
  } else {
    cb(null, false);
  }
}

app.use(multer({storage: fileStorage, fileFilter: fileFilter}).single('image'));
```

### Storing File Data in the Database

we can not store all files in the database cause it's too inefficient to store them in a database and query them from there. But we could store the path to the file.

```javascript
const image = req.file;
const imageUrl = image.path;
```

### Serving Images Statically

​		app.js

```javascript
app.use('/images', express.static(path.join(__dirname, 'images')));
```

​		index.ejs (add a '/' )

```javascript
 <img src="/<%= product.imageUrl %>"
```

### Downloading Files

​		/routes/shop.js

```javascript
router.get('/orders/:orderId', isAuth, shopController.getInvoice);
```

​		/controllers/shop.js

```javascript
exports.getInvoice = (req, res, next) => {
  const orderId = req.params.orderId;
  const invoiceName = 'invoice-' + orderId + '.pdf';
  const invoicePath = path.join('data', 'invoices', invoiceName);
  fs.readFile(invoicePath, (err, data) => {
    if(err) {
      return next(err);
    }
    // send file to browser
    res.setHeader('Content-Type', "application/pdf");
    // define filename
    res.setHeader('Content-Disposition', 'attachment; filename="' + invoiceName +'"'); 
    // attachment can be replaced by 'inline', which opens the file directly in the browser
    res.send(data);
  });
};
```

​		/views/orders.ejs

```html
<a href="/orders/<%= order._id %>">Invoice</a>
```

### Streaming Data

​		`fs.readFile` read the entire file into memory, and then return it with the response. This means that for bigger files, this will take very long before a response is sent and your memory on the server might overflow. So we should **streaming** our responses.

```javascript
const file = fs.createReadStream(invoicePath);
res.setHeader('Content-Type', 'application/pdf');
res.setHeader(
  'Content-Disposition',
  'inline; filename="' + invoiceName + '"'
);
file.pipe(res); // response is a writable stream, we use readable stream (file) to pipe their output into a writable stream 
```

### Using PDFKit for .pdf Generation

`npm install --save pdfkit`

```javascript
const PDFDocument = require('pdfkit');
const pdfDoc = new PDFDocument();
const invoiceName = 'invoice-' + orderId + '.pdf';
const invoicePath = path.join('data', 'invoices', invoiceName);
pdfDoc.pipe(fs.createWriteStream(invoicePath));
pdfDoc.pipe(res);

pdfDoc.text('Hello World');
pdfDoc.end();
```

### Generating pdf with Data

```javascript
pdfDoc.fontSize(26).text('Invoice', {
  underline: true
});
pdfDoc.text('-------------------------');
let totalPrice = 0;
order.products.forEach(prod => {
  totalPrice += prod.quantity * prod.product.price;
  pdfDoc.fontSize(14).text(prod.product.title + ' - ' + prod.quantity + ' * ' + '$' + prod.product.price);
})
pdfDoc.text('---');
pdfDoc.fontSize(20).text('Total Price: $' + totalPrice);
pdfDoc.end();
```

### Deleting A File

```javascript
const deleteFile = (filePath) => {
    fs.unlink(filePath, (err) => {
        if(err) {
            throw (err);
        }
    })
}
```

# Pagination

​		/controllers/shop.js

```javascript
  const page = +req.query.page || 1;
  let totalItems;
  Product.find()
  .countDocuments() // 获取总数
  .then(numProducts => {
    totalItems = numProducts;
    return Product.find()
    .skip((page - 1) * ITEMS_PER_PAGE)
    .limit(ITEMS_PER_PAGE);
  })
    .then((products) => {
      res.render('shop/product-list', {
        prods: products,
        pageTitle: 'Products',
        path: '/products',
        currentPage: page,
        hasNextPage: ITEMS_PER_PAGE * page < totalItems,
        hasPreviousPage: page > 1,
        nextPage: page + 1,
        previousPage: page - 1,
        lastPage: Math.ceil(totalItems / ITEMS_PER_PAGE)
      });
    })
```

​		/views/pagination.ejs

```html
<section class="pagination">
    <% if (currentPage !== 1 && previousPage !== 1) { %>
        <a href="?page=1">1</a>
    <% } %>
    <% if (hasPreviousPage) { %>
        <a href="?page=<%= previousPage %>"><%= previousPage %></a>
    <% } %>
    <a href="?page=<%= currentPage %>" class="active"><%= currentPage %></a>
    <% if (hasNextPage) { %>
        <a href="?page=<%= nextPage %>"><%= nextPage %></a>
    <% } %>
    <% if (lastPage !== currentPage && nextPage !== lastPage) { %>
        <a href="?page=<%= lastPage %>"><%= lastPage %></a>
    <% } %>
</section>
```

# Async Requests

### Definition

**Sync**: we send a request from client to the server and we get back a response which always was a html page or a redirect to another route that then returned a html page.

**Async**: we send the request but that request typically contains just some data in a format named `json` and that data is sent to the server, to a certain url or a route. And then the server returns a response behind the scenes. So it's not a new html page but again just some data in json format.

![nodejs43](img/nodejs43.jpeg)

### Application



/views/products.ejs

- before

![nodejs44](img/nodejs44.jpeg)

we don't need a `form` anymore, which sends a request with x-www-form-urlencoded data, instead we gather the data manually.

- after

![nodejs45](img/nodejs45.jpeg)

/public/js/admin.js

![nodejs46](img/nodejs46.jpeg)

/routes/admin.js

```javascript
router.delete('/product/:productId', isAuth, adminController.deleteProduct);
```

/controllers/admin.js

```javascript
exports.deleteProduct = (req, res, next) => {
  const prodId = req.params.productId;
  Product.findById(prodId).then(product => {
    if(!product) {
      return next(new Error('Product not found!'));
    }
    fileHelper.deleteFile(product.imageUrl);
    return Product.deleteOne({_id: prodId, userId: req.user._id});
  })
    .then(() => {
      console.log('DESTROYED PRODUCT');
      res.status(200).json({ message: 'Success!'});
    })
    .catch((err) => {
      res.status(500).json({ message: 'Deleting product failed.' });
    });
};
```

# REST APIs

Decoupling Frontend and Backend

### Server Side

app.js

```javascript
const express = require('express');

const feedRoutes = require('./routes/feed');

const app = express();

app.use(express.json()); // application/json

// 解决跨域问题
app.use((req, res, next) => {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, PATCH, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
    next();
});

app.use('/feed', feedRoutes);

app.listen(8080);
```

/routes/feed.js

```javascript
const express = require('express');

const feedController = require('../controllers/feed');

const router = express.Router();
// GET /feed/posts
router.get('/posts', feedController.getPosts);
// POST /feed/post
router.post('/post', feedController.createPost);

module.exports = router;
```

/controllers/feed.js

```javascript
exports.getPosts = (req, res, next) => {
    res.status(200).json({
        posts: [{ title: 'First Post', content: 'This is the first post!' }]
    });
};

exports.createPost = (req, res, next) => {
    const title = req.body.title;
    const content = req.body.content;
    console.log(title, content);
    res.status(201).json({
        message: 'Post created successfully!',
        post: {
            id: new Date().toISOString(),
            title: title,
            content: content
        }
    })
};
```

### Client Side (with Codepen)

html

```html
<button id="get">Get Posts</button>
<button id="post">Create a  Post</button>
```

JS

```js
const getButton = document.getElementById('get');
const postButton = document.getElementById('post');

getButton.addEventListener('click', () => {
  fetch('http://localhost:8080/feed/posts')
  .then(res => res.json())
  .then(resData => console.log(resData))
  .catch(err => console.log(err));
});

postButton.addEventListener('click', () => {
  fetch('http://localhost:8080/feed/post', {
    method: 'POST',
    body: JSON.stringify({
      title: 'A codepen post', 
      content: 'created via codepen'
    }),
    headers: {
      'Content-Type': 'application/json'
    }
  })
  .then(res => res.json())
  .then(resData => console.log(resData))
  .catch(err => console.log(err));
});
```

### Fetching Data

- Server Side

  /routes/feed.js

```javascript
// GET /feed/posts
router.get('/posts', feedController.getPosts);
```

​				/controllers/feed.js

```javascript
const Post = require('../models/post');
exports.getPosts = (req, res, next) => {
  Post.find()
    .populate('creator')
    .sort({ createdAt: -1 }) // sort by desc
    .then(posts => {
        res.status(200).json({
            message: 'Fetched posts successfully.',
            posts: posts
        });
    })
    .catch((err) => {
      if (!err.statusCode) {
        err.statusCode = 500;
      }
      next(err);
    });
};
```

- Client Side

  ```javascript
  fetch('http://localhost:8080/feed/posts')
        .then(res => {
          if (res.status !== 200) {
            throw new Error('Failed to fetch posts.');
          }
          return res.json();
        })
        .then(resData => {
          this.setState({
            posts: resData.posts,
            totalPosts: resData.totalItems,
            postsLoading: false
          });
        })
        .catch(this.catchError);
    };
  ```

  

### Creating Data

- Server Side

  Schema

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const postSchema = new Schema(
  {
    title: {
      type: String,
      required: true,
    },
    imageUrl: {
      type: String,
      required: true,
    },
    content: {
      type: String,
      required: true,
    },
    creator: {
      type: Object,
      required: true,
    },
  },
  {
    timestamps: true, // get createdAt, updatedAt automatically
  }
);

module.exports = mongoose.model('Post', postSchema);
```

​				/routes/feed.js

```javascript
// POST /feed/post
router.post(
  '/post',
  // sercer side validation
  [
    body('title').trim().isLength({ min: 5 }),
    body('content').trim().isLength({ min: 5 }),
  ],
  feedController.createPost
);
```

​				/controllers/feed.js

```javascript
exports.createPost = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    const error = new Error('Validation failed, entered data is incorrect.');
    error.statusCode = 422;
    throw error;
  }
  if(!req.file) {
    const error = new Error('No image provided.');
    error.statusCode = 422;
    throw error;
  }
  const imageUrl = req.file.path;
  const title = req.body.title;
  const content = req.body.content;
  const post = new Post({
    title: title,
    content: content,
    imageUrl: imageUrl,
    creator: { name: 'biyun' },
  });
  post
    .save()
    .then((result) => {
      res.status(201).json({
        message: 'Post created successfully!',
        post: post,
      });
    })
    .catch((err) => {
      if (!err.statusCode) {
        err.statusCode = 500;
      }
      next(err);
    });
};
```

- Client Side

   (without uploading image)

  ![nodejs47](img/nodejs47.jpeg)

  `application/json`  represent only text data but not file, so we need form data.



​				(setup images)

![nodejs48](img/nodejs48.jpeg)

​			we don't need to set the Headers, cause the form data will be set automatically.

### Fetching Single Data

- Server Side

  /routes/feed.js

  ```javascript
  router.get('/post/:postId', feedController.getPost);
  ```

  /controllers/feed.js

  ```javascript
  exports.getPost = (req, res, next) => {
    const postId = req.params.postId;
    Post.findById(postId)
      .then((post) => {
        if (!post) {
          const error = new Error(
            'Validation failed, entered data is incorrect.'
          );
          error.statusCode = 422;
          throw error;
        }
        res.status(200).json({ message: 'Post fetched.', post: post });
      })
      .catch((err) => {
        if (!err.statusCode) {
          err.statusCode = 500;
        }
        next(err);
      });
  };
  ```

- Client Side

  ```javascript
  const postId = this.props.match.params.postId;
      fetch('http://localhost:8080/feed/post/' + postId)
        .then(res => {
          if (res.status !== 200) {
            throw new Error('Failed to fetch status');
          }
          return res.json();
        })
        .then(resData => {
          this.setState({
            title: resData.post.title,
            author: resData.post.creator.name,
            image: 'http://localhost:8080/' + resData.post.imageUrl,
            date: new Date(resData.post.createdAt).toLocaleDateString('en-US'),
            content: resData.post.content
          });
        })
        .catch(err => {
          console.log(err);
        });
  ```

  

### Updating Data

- Server Side

  /routes/feed.js  ( **put** )

  ```javascript
  router.put('/post/:postId', feedController.editPost);
  ```

  /controllers/feed.js

  ```javascript
  exports.editPost = (req, res, next) => {
    const postId = req.params.postId;
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      const error = new Error('Validation failed, entered data is incorrect.');
      error.statusCode = 422;
      throw error;
    }
    const title = req.body.title;
    const content = req.body.content;
    let imageUrl = req.body.image;
  
    if (req.file) {
      imageUrl = req.file.path;
    }
    if (!imageUrl) {
      const error = new Error('No image picked.');
      error.statusCode = 422;
      throw error;
    }
    Post.findById(postId)
      .then((post) => {
        if (!post) {
          const error = new Error(
            'Can not find post.'
          );
          error.statusCode = 404;
          throw error;
        }
        if(imageUrl !== post.imageUrl) {
            clearImage(post.imageUrl);
        }
        post.title = title;
        post.content = content;
        post.imageUrl = imageUrl;
        return post.save();
      })
      .then(result => {
          res.status(200).json({message: 'Post updated!', post: result});
      })
      .catch((err) => {
        if (!err.statusCode) {
          err.statusCode = 500;
        }
        next(err);
      });
  };
  
  const clearImage = filePath => {
      filePath = path.join(__dirname, '..', filePath);
      fs.unlink(filePath, err => console.log(err));
  };
  ```

- Client Side

  ![nodejs49](img/nodejs49.jpeg)

### Deleting Data

- Server Side

  /routes/feed.js

  ```javascript
  router.delete('/post/:postId', feedController.deletePost);
  ```

  /controllers/feed.js

  ```javascript
  exports.deletePost = (req, res, next) => {
      const postId = req.params.postId;
      Post.findById(postId)
        .then((post) => {
          if (!post) {
            const error = new Error(
              'Can not find post.'
            );
            error.statusCode = 404;
            throw error;
          }
          // check logged in user 
          clearImage(post.imageUrl);
          return Post.findByIdAndRemove(postId);
        })
        .then(result => {
            console.log(result);
            res.status(200).json({ message: 'Post deleted.' });
        })
        .catch((err) => {
          if (!err.statusCode) {
            err.statusCode = 500;
          }
          next(err);
        });
    };
  
  ```

- Client Side

![nodejs50](img/nodejs50.jpeg)

### Authentication

### Sign up

​		/routes/auth.js

```javascript
router.put( // both post and put are fine
  '/signup',
  [
    body('email')
      .isEmail()
      .withMessage('Please enter a valid email.')
      .custom((value, { req }) => {
        return User.findOne({ email: value }).then((userDoc) => {
          if (userDoc) {
            return Promise.reject('E-mail address already exists!');
          }
        });
      })
      .normalizeEmail(),
    body('password').trim().isLength({ min: 5 }),
    body('name').trim().not().isEmpty(),
  ],
  authController.signup
);
```

​		/controllers/auth.js

```javascript
exports.signup = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    const error = new Error('Validation failed.');
    error.statusCode = 422;
    error.data = errors.array();
    throw error;
  }
  const email = req.body.email;
  const name = req.body.name;
  const password = req.body.password;
  bcrypt
    .hash(password, 12)
    .then((hashedPw) => {
      const user = new User({
        email: email,
        name: name,
        password: hashedPw,
      });
      return user.save();
    })
    .then((result) => {
      res.status(201).json({ message: 'User created!', userId: result._id });
    })
    .catch((err) => {
      if (!err.statusCode) {
        err.statusCode = 500;
      }
      next(err);
    });
};
```

With a session, the server needs to store data about the client, and then stores that a client is authenticated.

RESTful APIs are stateless, they don't care about the client. Every request should have all the data it needs to authenticate itself.

![nodejs51](img/nodejs51.jpeg)

`npm install --save jsonwebtoken`

### Log in

​		/routes/auth.js

```javascript
router.post('/login', authController.login);
```

​		/controllers/auth.js

```javascript
exports.login = (req, res, next) => {
  const email = req.body.email;
  const password = req.body.password;
  let loadedUser;
  User.findOne({ email: email })
    .then((user) => {
      if (!user) {
        const error = new Error('A user with this email could not be found.');
        error.statusCode = 401;
        throw error;
      }
      loadedUser = user;
      return bcrypt.compare(password, user.password);
    })
    .then((isEqual) => {
      if (!isEqual) {
        const error = new Error('Wrong password.');
        error.statusCode = 401;
        throw error;
      }
      // create a new signature and pack ot into a new json web token
      const token = jwt.sign(
        {
          email: loadedUser.email,
          userId: loadedUser._id.toString(),
        },
        'somesupersecretsecret', //private key
        { expiresIn: '1h' }
      );
      res.status(200).json({ token: token, userId: loadedUser._id.toString() });
    })
    .catch((err) => {
      if (!err.statusCode) {
        err.statusCode = 500;
      }
      next(err);
    });
};

```

### Using Token

For each request, we add a token for them.

- ​	Client Side

```javascript
fetch('http://localhost:8080/feed/posts?page=' + page, {
      headers: {
        Authorization: 'Bearer ' + this.props.token
      }
    })
```

- Server Side

  /middleware/is-auth.js

  ```javascript
  const jwt = require('jsonwebtoken');
  
  module.exports = (req, res, next) => {
      const authHeader = req.get('Authorization');
      if(!authHeader) {
          const error = new Error('Not authenticated');
          error.statusCode = 401;
          throw error;
      }
      const token = authHeader.split(' ')[1];
      let decodedToken;
      try {
          decodedToken = jwt.verify(token, 'somesupersecretsecret');
      } catch (err) {
          err.statusCode = 500;
          throw err;
      }
      if(!decodedToken) {
          const error = new Error('Not authenticated');
          error.statusCode = 401;
          throw error;
      }
      req.userId = decodedToken.userId;
      next();
  };
  ```

  /routes/feed.js

  ```javascript
  const isAuth = require('../middleware/is-auth');
  router.get('/posts', isAuth, feedController.getPosts);
  ```

### Connecting Relations

- When create a post, it should be associated with user.

  ```javascript
  post
      .save()
      .then((result) => {
        return User.findById(req.userId);
      })
      .then((user) => {
        creator = user;
        user.posts.push(post);
        return user.save();
      })
  ```

  When deleting a post...

  ```javascript
   .then((result) => {
        return User.findById(req.userId);
      })
      .then(user => {
        user.posts.pull(postId);
        return user.save();
      })
  ```

  

# Web Socket

With http, client send a request to the server, and the server return a response. But if  server want to inform something to client, we need to use `web socket` which build up on http.

### Setup

- Server Side

  `npm install --save socket.io`

  ​		socket.js

  ```javascript
  let io;
  
  module.exports = {
      init: httpServer => {
          io = require('socket.io')(httpServer, {
              cors: {
                  origin: 'http://localhost:3000',
                  methods: ['GET', 'POST']
              }
          });
          return io;
      },
      getIO: () => {
          if(!io) {
              throw new Error('Socket.io not initialized!');
          }
          return io;
      }
  };
  ```

  ​		app.js

  ![nodejs52](img/nodejs52.jpeg)

- Client Side

  `npm install --save socket.io-client`

```javascript
import openSocket from 'socket.io-client';
openSocket('http://localhost:8080');
```

With **Web Socket**, when a user update the database, the other users don't need to reload the page and they get the update automatically.

### Synchronizing

- Server Side (createPost)

  `const io = require('../socket');`

![nodejs53](img/nodejs53.jpeg)

- Client Side

  ![nodejs54](img/nodejs54.jpeg)

# GraphQL

`npm install --save graphql express-graphql`

We don't need routes any more.

### Setup & First query

​		/graphql/schema.js

```javascript
const { buildSchema } = require('graphql');

module.exports = buildSchema(`
    type TestData {
        text: String!
        views: Int!
    }
    type RootQuery {
        hello: TestData!
    }
    schema {
        query: RootQuery
    }
`);
```

​		/graphql/resolvers.js

```javascript
module.exports = {
    hello() {
        return {
            text: 'Hello World!',
            views: 1234
        };
    }
}
```

​		app.js

```javascript
const { graphqlHTTP } = require('express-graphql');

const graphqlSchema = require('./graphql/schema');
const graphqlResolver = require('./graphql/resolvers');

app.use(
  '/graphql',
  graphqlHTTP({
    schema: graphqlSchema,
    rootValue: graphqlResolver,
  })
);
```

​		postman

![nodejs55](img/nodejs55.jpeg)

### Defining a Mutation Schema

/graphql/schema.js

```javascript
const { buildSchema } = require('graphql');

module.exports = buildSchema(`
    type Post {
        _id: ID!
        title: String!
        content: String!
        imageUrl: String!
        creator: User!
        createdAt: String!
        updatedAt: String!
    }
    type User {
        _id: ID!
        name: String!
        email: String!
        password: String
        status: String!
        posts: [Post!]!
    }
    input UserInputData {
        email: String!
        password: String!
        name: String!
    }
    type RootQuery {
        hello: String
    }
    type RootMutation {
        createUser(userInput: UserInputData): User!
    }
    schema {
        query: RootQuery 
        mutation: RootMutation
    }
`);
```

​		Query root type must be provided.

### Mutation Resolver

​		`npm install --save validator`

```javascript
const User = require('../models/user');
const bcrypt = require('bcryptjs');
const validator = require('validator');

module.exports = {
  createUser: async function ({ userInput }, req) {
    // validation
    const errors = [];
    if (!validator.isEmail(userInput.email)) {
      errors.push({ message: 'E-mail is invalid.' });
    }
    if (
      validator.isEmpty(userInput.password) ||
      validator.isLength(userInput.password, { min: 5 })
    ) {
        errors.push({ message: 'Password too short!' });
    }
    if(errors.length > 0) {
        const error = new Error('Invalid input.');
        error.data = errors;
        error.code = 422
        throw error;
    }
    // end validator
    const existingUser = await User.findOne({ email: userInput.email });
    if (existingUser) {
        const error = new Error('User exists already!');
        throw error;
    }
    const hashedPw = await bcrypt.hash(userInput.password, 12);
    const user = new User({
      email: userInput.email,
      name: userInput.name,
      password: hashedPw,
    });
    const createdUser = await user.save();
    return {
      ...createdUser._doc,
      _id: createdUser._id.toString(),
    };
  },
};

```

### Tool GraphiQL

​		app.js

```javascript
app.use(
  '/graphql',
  graphqlHTTP({
    schema: graphqlSchema,
    rootValue: graphqlResolver,
    // necessary for tool GraphiQL
    graphiql: true,
    customFormatErrorFn(err) {
      if(!err.originalError) { // originalError: thrown by ourselves or third party package
        return err;
      }
      const data = err.originalError.data;
      const message = err.message || 'An error occurred.';
      const code = err.originalError.code || 500;
      return { message: message, status: code, data: data };
    }
  })
);
```

==> http://localhost:8080/graphql

![nodejs56](img/nodejs56.jpeg)

### Connecting the Frontend to the GraphQL API

- **signupHandler**

```javascript
const graphQuery = {
      query: `
        mutation {
          createUser(userInput: {email: "${authData.signupForm.email.value}", name: "${authData.signupForm.name.value}", password: "${authData.signupForm.password.value}" }) {
            _id
            email
          }
        }
      `,
    };
    fetch('http://localhost:8080/graphql', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(graphQuery)
    })
      .then((res) => {
        return res.json();
      })
      .then((resData) => {
        if (resData.errors && resData.errors[0].status === 422) {
          throw new Error(
            "Validation failed. Make sure the email address isn't used yet!"
          );
        }
        if (resData.errors) {
          throw new Error('Creating a user failed!');
        }
        console.log(resData);
        this.setState({ isAuth: false, authLoading: false });
        this.props.history.replace('/');
      })
```

Now, we get an error.

![nodejs57](img/nodejs57.jpeg)

The browser sends an `OPTIONS` request before it sends the `POST`, `PATCH`, `PUT`, `DELETE` or so on request. Express-graphql automatically declines anything which is not a `POST` or `GET` request, so the `OPTIONS` request is denied.

`Solution`:

In the middleware for cors and headers:

```javascript
if(req.method === 'OPTIONS') {
    return res.sendStatus(200);
  }
```

![nodejs58](img/nodejs58.jpeg)

### Using Variable

```javascript
let page = 1;
const graphqlQuery = {
      query: `
        query FetchPosts($currentPage: Int){
          posts(page: $currentPage) {
            posts {
              _id
              title
              content
              imageUrl
              creator {
                name
              }
              createdAt
            }
            totalPosts
          }
        }
      `,
      variables: {
        currentPage: page
      }
    };
```

### Summary

Queries( GET )

Mutation ( POST, PUT, PATCH, DELETE )

# Deployment

### Environment Variable

 => nodemon.json

```json
{
    "env": {
        "NODE_ENV": "production",
        "MONGO_USER": "biyun",
        "MONGO_PASSWORD": "RVx1COV1vf3RxT8Z",
        "MONGO_DEFAULT_DATABASE": "shop"
    }
}
```

=> app.js

![nodejs61](img/nodejs61.jpeg)

### Secure Response Headers

`npm install --save helmet`

=> app.js

`app.use(helmet())`

### Compressing Assets

`npm install --save compression`

`app.use(compression())`



### Setting up Request Logging

`npm install --save morgan`

```javascript
const accessLogStream = fs.createWriteStream(
  path.join(__dirname, 'access.log'),
  { flags: 'a' } // a: append
);
app.use(morgan('combined', { stream: accessLogStream }));
```

​	把log写入文件	

### Setting up SSL Server

`openssl req -nodes -new -x509 -keyout server.key -out server.cert`

```javascript
const https = require('https');
const privateKey = fs.readFileSync('server.key');
const certificate = fs.readFileSync('server.cert');
// ...
https
  .createServer({ key: privateKey, cert: certificate }, app)
  .listen(process.env.PORT || 3000);
```

==> browser: https://localhost:3000

==> If error: You cannot visit localhost right now because the website sent scrambled credentials that Google Chrome cannot process. Network errors and attacks are usually temporary, so this page will probably work later.



==> **Solution**: Just type "thisisunsafe" in chrome.

### Hosting Provider

Heroku

# Testing

![nodejs59](img/nodejs59.jpeg)



### Set up

`npm install --save-dev mocha chai`

package.json

`"test": "mocha"` 

/test/start.js

```js
const expect = require('chai').expect;
it('should add numbers correctly', () => {
    const num1 = 2;
    const num2 = 3;
    expect(num1 + num2).to.equal(5);
})
```

`Mocha` is responsible for running our tests and for giving us this `IT` function that defines where we define our test code.

`Chai` is responsible for defining our success conditions and for this we just need to import something from `Chai`.

![nodejs60](img/nodejs60.jpeg)

