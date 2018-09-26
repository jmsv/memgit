# memgit

uses [isomorphic-git](https://github.com/isomorphic-git/isomorphic-git) and [memfs](https://github.com/streamich/memfs) to host an in-memory git repo

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
