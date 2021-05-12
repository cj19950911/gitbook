# react-admin Redux废弃

## 概括
React-adminを削除して、対応するべきところは下記の対応が必要です           
1.多言語対応、React-adminからReact-intlに切り替えます。       
-> 対応完了、useTranslateの仕組みをこのままで、内部ロジックを自分で書きます。       
2.Router機能、connected-react-routerで対応します。         
-> 対応完了      
3.権限管理、App.tsxでuseEffectで対応できるけど、もっと良い案があるかもしれません。        
→       
ReactのContextで対応します、問題なさそうです。          
4. 既存のResourceとFieldのコードの再構築。        
→      
対応中です、何か誰も簡単にコードを再構築できるのDemoを対応中。       
一番理想の対応は、コードの対応が必要ない、自分のReduxだけ修正して対応します。       
5.Reduxの対応       
→     
Resource以外の部分のReduxを対応完了       

## React-admin以前的Redux是干啥
### Router
- Router機能、Connected-router用它，将Router绑定到Store里
- 与一般Router不同，传入Props的History有两种方式，Connect（History）和Router的Props传参，而Router本体是WithRouter（）和Router的Props传参
- 决定用回一般Router
### 権限
- 权限设定、原来的react-admin有两重设定、auth和permission，auth因为原来已经在app.tsx的层级上判定了，属于doublecheck，只保留app.tsx的层级的authCheck，
- 至于permissionCheck，事实上，由于Permission的返回值过于单一，导致很多逻辑必须通过LocalStorage实现，这边考虑的是用Axios和Props来控制。
- usePermission废止，useAuthCheck保留
- 最终是用createContext和useContext实现跨层传输
- 可访问判断-路由判断-LocalStorage
- 部分可访问判断-如多语言部分-以前有延迟问题（本身是usePermission的设计问题），统一在Page端返回
### 数据端（dataProvider）
- 原有的Templete和Page端，不变，只是将以前依存react-admin的useHook去除，附加到自己的Context上
- 原有的Redux逻辑，放弃
- Redux只用于编辑和登录页面
- 预想的Redux结构（大概率会改动不少）
```

export type Tab = {
  tabValue: JSONObject;
  name:string;
  error: boolean;
  validate:JSONObject;//{组件name:boolean....}
};

export type State = {
  status:string;
  allowSubmit:boolean;
  title:string;
  id:string;
  value: JSONObject;
  disabledValue?: Group[];
  tabs: tab[];
};
````
- 组件与Container联通的有onChange和value和onValidate 3项
```
input.value= useContainer().state.value["resource.name"]
input.onChange=useContainer().onChange(value,resource.name,tab的下标ID)
```
- onChange的内部会触发onValidate的函数，一旦结果为负，state中该tab的validate的值更新为负，一旦结果为正，将该validate的值更新为正，与此同时刷新tab的error，如若tab的error变化，刷新State的あallowSubmit
- 提交按钮是否可用取决于state.allowSubmit
- tab是否标红取决于该state.tab[].error
### menu控制
- State 控制
