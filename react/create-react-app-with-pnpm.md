# create react app with pnpm


```shell
pnpm dlx create-react-app ./my-app
```

https://pnpm.io/ja/cli/dlx


pnpmだけを使うようにする
package.json
```json
{
    "scripts": {
        "preinstall": "npx -y only-allow pnpm"
    }
}
```

https://pnpm.io/ja/only-allow-pnpm