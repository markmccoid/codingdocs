## variables.env

It is good to keep certain variables, like database path, username, password, keys, etc in a separate file outside of your js files that get saved to github or published to production.

Sample file might look like:
```
NODE_ENV=development
DATABASE=mongodb://mark:lorimcco@ds135552.mlab.com:35552/dang-thats-delicious
MAIL_USER=123
MAIL_PASS=123
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
PORT=7777
MAP_KEY=AIzaSyD9ycobB5RiavbXpJBo0Muz2komaqqvGv0
SECRET=snickers
KEY=sweetsesh
```
To read this file in, you can use the **dotenv** npm module.

```javascript
require('dotenv').config({ path: 'variables.env' });

mongoose.connect(process.env.DATABASE);
```
