# realm-how-to

Once the branch is pulled onto your machine:
```
npm install
expo run:ios -d
```

^^This will take a while!

Then to run
```
npm start
```

```javascript
// .models/Task.js

//Realm schemas are in classes. This is an example for a Task collection:

import { Realm } from "@realm/react";

export class Task {
  constructor({
    id = new Realm.BSON.ObjectId(),
    name
  }) {
    this.name = name;
    this._id = id;
  }

// new Realm.BSON.ObjectID() assigns a unique MongoDB object Id to the entry

  static schema = {
    name: "Task",
    primaryKey: "_id",
    properties: {
      name: "string",
      _id: "objectId",
    },
  };
}
```
