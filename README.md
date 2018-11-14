# memgit

__uses [isomorphic-git](https://github.com/isomorphic-git/isomorphic-git) and [memfs](https://github.com/streamich/memfs) to host an in-memory git repo!__

(At the moment it's just a proof-of-concept - I'm not sure what to do about git's compressed objects although there's probably a way to magically store them as text)

#### why did u make this tho

I don't have much use for this and made it for fun tbh, although it could potentially be useful for storing a git repo in a document-based (e.g. MongoDB) database

In the future (given time and motivation) I might get this working properly and turn it into something more reusable

---

The code I wrote trying to get this to work:

```js
var fsReal = require('fs')

var fs = require('memfs').fs
var vol = require('memfs').vol

var git = require('isomorphic-git')

git.plugins.set('fs', fs)

async function main () {
  await git.init({ dir: '/' })

  fs.writeFileSync('/avocado.txt', 'avocado content')

  await git.add({dir: '/', filepath: '/avocado.txt'})

  await git.commit({
    dir: '/',
    author: {
      name: 'James Vickery',
      email: 'dev@jamesvickery.net'
    },
    message: 'added avocado'
  })

  const repoJson = vol.toJSON()

  fsReal.writeFileSync('repo.json', JSON.stringify(repoJson, null, 2))
}

main()
```

![output](img/output.png)
