# realm-how-to

To install realm:
```
npm install @realm/react
expo run:ios -d
```

^^This will take a while!

Then to run
```
npm start
```

## Creating Realm collections

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

## Linking the schemas with Realm

```javascript
// ./components/createRealmContext.js

import { createRealmContext } from "@realm/react";
import { Task } from "./models/Task";
export const { useRealm, useQuery, RealmProvider } = createRealmContext({
  schema: [Task.schema],
});

// useRealm connects a React component to Realm.

// useQuery selects all objects in a collection
// eg. all Task objects:
const tasks = realm.useQuery("Task");

// RealmProvider is a component wrapper for the entire app to allow Realm to work within it (see below).
```

## Using it in App.js

```javascript
//app.js

import { useRealm, RealmProvider } from "./components/CreateRealmContext";

const App = () => {
  const realm = useRealm();

return...
}
```
Realm Provider is a wrapper that allows Realm functionality within components.
It must wrap the entire app to work.
```javascript
export default function AppWrapper() {
  return (
    <RealmProvider>
      <App />
    </RealmProvider>
  );
}
```

## Querying Realm Collections

To do the following it must be in a component where this is called:
```javascript
import { useRealm } from "./models/Task";

const realm = useRealm()
```

To find an entry in a Realm collection:

eg. finding the entry with the name "Newest task" in the Task collection:

```javascript
realm.objects("Task").filtered("name == $0", "Newest task")

// $0 refers to the argument after the search query

"name == $0", "Newest task"
// search by name, where name is "Newest task"

"name == $0, priorityLevel > $1", "Newest task", "5"
//s earch by name, where name is "Newest task" and priorityLevel is 5
```

Realm 'transactions' are a way of making insert, update, and delete queries to Realm collections.
It is done as a callback function:

```javascript
realm.write(() => {

// query goes here

})
```

Creating a new entry:
```javascript
import { Task } from "./models/Task"

realm.write(() => {

    realm.create(
        "Task",
        new Task({ name: "New entry" })
    )

})
```
Deleting a specific entry:
eg. from the Task collection with the name "Newest task"
```javascript
realm.write(()=> {

    realm.delete(
        realm.objects("Task").filtered("name == $0", "Newest task")
    )

})
```
Deleting all entries in database:
```javascript
realm.write(() => {

    realm.deleteAll()

})
```

