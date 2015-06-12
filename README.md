UPDATE 06/10/2015: Added my "faster_bson.js" file, leading to a (by my tests) faster all-JS parser than the current "Pure" version; presumably, the "native" C++ parser is ~2x as fast as the original JS-BSON, however I also wanted to do away with deliberately thrown errors in the parser that necessitated V8-de-optimizing try/catch's in the driver code, and would rather not wade into C++ to do so. Hopefully this now puts the ( **node.js-ONLY, depends/assumes 100% node/Buffer; no browser usage!** ) parser close to the C++ performance.

Making the tightened for my purposes (left out older/unused/deprecated features/BSON classes and all code/serializeFunctions) faster_bson.js **available as an example/food-for-thought item ONLY, ZERO guarantee of completeness or fitness for your purposes.** It also needs to be set up a good bit differently in the (also customized) driver, with a plain "bson = require('./faster_bson')".

--- ---

# Description
"...The MongoDB Core driver is the low level part of the 2.0 or higher MongoDB driver and is meant for library developers not end users. It does not contain any abstractions or helpers outside of the basic management of MongoDB topology connections, CRUD operations and authentication."
---

As it should be... I've been working on reducing this entire "core" driver to an even smaller minimalist subset of 3(!) files at about 200 lines each. This repo is only serving as a quick review of the original.

Also working on a rewrite of the GridStore methodology, as frankly, despite the overall benefits of an FS-less, DB-centric approach, the 2-step query lookup of the current GridFS is far too costly in terms of performance. See here:
github.com/feifangit/MongoDB-GridFS-test (of course, some of this is also sheer Express slowness - tends to cost 50% raw Node.js perf right off the bat... ugh...)

While looking into that issue, I also noticed how much performance the std mongodb-node-native (pre-2.0) driver was leaving on the table with a long cascade of options checking/merging (many of them for mere backwards compatibility...), and a near dozen step cascade of calls and object instantiations before ever arriving at the actual MongoDB command.
