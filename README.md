
# Search Filters MongoDB

search-filters compiler for mongodb targets

## Installation

```sh
npm install --save @quenk/search-filters-mongodb
```

## Usage

This module provides a [@quenk/search-filters][1] compiler for generating
mongodb [query operators][2]. It targets the [Node.js Driver API][3] particularly
the `Collection#find` method.

To compile a source string, create an instance of `MongoDBFilterCompiler`
and apply the `compile()` method giving it an [EnabledPolicies][4] and 
a valid source string.

### Available Policies

This module ships with the following [Available Policies][5] :

| Policy Name    | Type         | Operators      | Notes                       |
|----------------|--------------|--------------- |-----------------------------|
| number         | number       | = < > >= <= != |                             |
| boolean        | boolean      | = < > >= <= != |                             |
| string         | string       | = !=           | Tests equality.             |
| match          | string       | =              | Converts value to regex.    |
| matchci        | string       | =              | Case-insensitive "match".   |
| date           | date         | = < > >= <= != | Actual value instanceof Date|    

You can specify a policy name instead of a definition in `EnabledPolicies`
and it will be subsitituted at compile time. 
Consult the [search-filters][1] docs for more information.

You can add additional `AvailablePolicies` by overriding the `policies`
argument of the `MongoDBFilterCompiler`.

### Example

```typecript
import {Value} from '@quenk/noni/lib/data/jsonx';
import {MongoDBFilterCompiler} from '@quenk/search-filters-mongodb';
import {FilterTerm} from '@quenk/search-filters-mongodb/lib/term';

const policies = {

 name: 'matchci',

 age: 'number'

 status: {

   type: 'number',
   operators: ['='],
   term: (field:string, op:string, value:Value ) => 
       new FilterTerm(field, op, value)

 }

}

const qry = '(name:"sana" or name:="Murr") and age:>20 and status:1';

let mfc = new MongoDBFilterCompiler();

let eResult = mfc.compile(qry);

console.error(eResult.takeRight()); // Throws an error if compilation failed.

```

## License

Apache 2.0 (SEE LICENSE) file. (c) Quenk Technologies Limited.

[1]: https://quenktechnologies.github.io/search-filters
[2]: https://docs.mongodb.com/manual/reference/operator/query/
[3]: https://mongodb.github.io/node-mongodb-native/3.5/api/
[4]: https://quenktechnologies.github.io/search-filters/interfaces/_compile_policy_.enabledpolicies.html
[5]: https://quenktechnologies.github.io/search-filters/interfaces/_compile_policy_.availablepolicies.html
