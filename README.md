# father monorepo 打包测试

## Usage

```bash
# install dependencies
$ pnpm install
```

根目录运行`pnpm run build`，查看`packages/bar/dist/esm/index.d.ts`，值为：

```ts
/// <reference types="react" />
export declare const bar = 1;
declare const widgets: {
  Foo: {
    component: import('react').FC<import('packages/foo/dist/esm').FooProps>;
  };
};
export default widgets;
```

修改`packages/bar/tsconfig.json`，添加`baseUrl:'.'`，重新 build，查看`packages/bar/dist/esm/index.d.ts`，值为：

```ts
/// <reference types="react" />
export declare const bar = 1;
declare const widgets: {
  Foo: {
    component: import('react').FC<import('@examples/foo').FooProps>;
  };
};
export default widgets;
```

打包结果正确，但此时开发模式下`@examples/foo` ts 提示指向`packages/foo/dist/esm/index.d.ts`，预期是想基于最外层的 paths 做提示解析。

一个比较好的解决方案是 build 的时候采用另外的 tsconfig.json。
