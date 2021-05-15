dx: A tool for writing better scripts with Deno
==========================================

# why a dx instead of zx

dx is based on Deno and with following pros:

* TypeScript friendly
* Easy to import third party modules, just `import {red, green} from "https://deno.land/std@0.95.0/fmt/colors.ts"`, no idea about zx to import third party npm(package.json???)
* I ❤️ 🦕 

# Install

```bash
deno install -A --unstable -r -f -n dx https://denopkg.com/linux-china/dx/cli.ts
```

# Demo

```typescript
#!/usr/bin/env dx

import {$, cd, pwd, question, os, fs, env} from "https://denopkg.com/linux-china/dx/mod.ts";
import {red, yellow, blue, green} from "https://deno.land/std@0.95.0/fmt/colors.ts";

// prompt to input your name
let name = await question(blue("what's your name: "));
console.log("Hello ", blue(name ?? "guest"));

console.log("Current working directory:", pwd());
console.log("Your home:", HOME);
console.log("Your name:", USER);

// language=bash count files
const output = await $`ls -1 | wc -l`;
console.log("Files count: ", parseInt(output));

// print your internet outbound ip
let json = await fetch('https://httpbin.org/ip').then(resp => resp.json());
console.log("Your ip: ", json.origin)
```

Then run `dx demo.ts` or `chmod u+x demo.ts ; ./demo.ts`'

# functions and variables

```typescript
import {$, cd, pwd, question, os, fs, env} from "https://denopkg.com/linux-china/dx/mod.ts";
```

* cd: change current working directory. `cd('../')` or `cd('~/')`
* pwd: get current working directory
* echo:  dump object as text on terminal
* printf:  format output
* cat:  read text file as string
* question: read value from stdin with prompt
* sleep: `await sleep(5);`  
* os: OS related functions
* fs: file system related functions
* glob:  glob files, like commands `ls -1 *.ts`
  
```typescript
// 
for await (const fileName of glob("*.ts")) {
    console.log(fileName);
}
```

* env: env variables
* Support to treat shell env variables as global variables in TypeScript

```typescript
// builtin env variables for hint, such as USER, HOME, PATH
const output = await $`ls -al  ${HOME}`;
console.log(HOME);

// custom env variables for hint
declare global {
    const JAVA_HOME: string;
}
```

# execute command

Same with zx, example as following:

```typescript
let count = parseInt(await $`ls -1 | wc -l`)
console.log(`Files count: ${count}`)
```

**Attention:**: if exit code is not 0, and exception will be thrown.

```typescript
try {
    await $`exit 1`
} catch (p) {
    console.log(`Exit code: ${p.exitCode}`)
    console.log(`Error: ${p.stderr}`)
}
```

# color output

Deno std has `fmt/colors.ts` already, and you don't need chalk for simple cases.

```typescript
import {red, yellow, blue, green} from "https://deno.land/std@0.95.0/fmt/colors.ts";

console.log(green("Hello"));
```

# $ configuration

$.shell and $.prefix are same to zx

# packages

fs and os packages are same to zx, and use fs and os modules from https://deno.land/std@0.95.0/node

# Misc

* .env auto load
* Compile script into executable binary: `deno compile --unstable -A --lite demo.ts`
* Language Injections in WebStorm:  Settings -> Editor -> Language Injections, and add "JS Tagged Literal Injection" as following:  

![Language Injections in WebStorm](./docs/webstorm-dx-settings.png)

# References

* Google zx: https://github.com/google/zx
