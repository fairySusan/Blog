### ts模块
  * 任何包含了import和export的文件都会被当成一个模块，那么这个文件里声明的变量方法除了export出去以外，外部是无法访问的。
  * 反之，如果一个文件不带有顶级的import或者export声明，那么它的内容被视为全局可见的（对模块也是可见的）

### ts模块的声明文件
  * 因为以前的很多库都没用ts编写，对非ts编写的模块文件的类型注解，叫声明文件，例如 global.d.ts
  ```ts
  declare module 'axios' {
    export interface AxiosResponse {}
    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
  }
  ```
  * 但是如果不想再使用一个新模块之前花时间去编写声明，可以采用声明的简写形式,这个模块里所有的类型都将是any
  ```TS
  declare module 'hot-new-module'
  ```
  * 在声明文件里对变量的声明,用declare关键字
    `declare var foo:number` or `declare let foo:number` or `declare const foo:number`
  * 对全局函数的声明
    `declare function greet(greeting:string): void`
  * 对带有属性的对象的声明
    js代码：
    ```JS
    myLib.greet()
    console.log(myLib.greeting)
    ```
    声明文件里：
    ```TS
    declare namespace myLib {
      function greet(s:string): string;
      let greeting:string;
    }
    ```
  * 对类的声明
    ```ts
    declare class Greeter {
      constructor(greeting:string);
      greeting:string;
      showGreeting():void;
    }
    ```