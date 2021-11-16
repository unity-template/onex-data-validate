针对服务端数据进行统一校验&转化，主要是针对组件使用过程中进行转化和封装，主要支持以下三种场景
* 类组件props
* 函数组件props
* 服务端数据的转化

### 使用说明
1. 类组件的props

```typescript
export class DataDto {
  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizCode: string;

  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizName: string;

  @Rule(RuleType.number())
  @addAlarm // 报警装饰
  note?: string;
}

@ValidateComponentProps(DataDto)
class Node extends Component<DataDto> {
  IRender() {
    console.log(this.props.bizCode, this.props.bizName, this.props.note);
  }
}

new Node({
  bizCode: '你好',
  bizName: '世界',
}).IRender();

/**
 * output:
 * Debugger attached.
 * DataDto.note: 参数告警
 * test-你好 test-世界 undefined
 */
```

2.函数组件的props
```typescript
export class DataDto {
  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizCode: string;

  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizName: string;

  @Rule(RuleType.number())
  @addAlarm // 报警装饰
  note?: string;
}

class Node extends Component<DataDto> {
  IRender() {
    console.log(this.props.bizCode, this.props.bizName, this.props.note);
  }
}

const INode = ValidateComponentPropsHoc(DataDto)(Node)

new INode({
  bizCode: '你好',
  bizName: '世界',
}).IRender();

/**
 * output:
 * Debugger attached.
 * DataDto.note: 参数告警
 * test-你好 test-世界 undefined
 */
```
3.服务端的数据的转化
```typescript
export class DataDto {
  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizCode: string;

  @Rule(RuleType.string().required())
  @addPre('test') // 添加前缀
  bizName: string;

  @Rule(RuleType.number())
  @addAlarm // 报警装饰
  note?: string;
}

const result = validateInterfaceData(DataDto)({
  bizCode: '你好',
  bizName: '世界',
  note: '213',
});

console.log(result.bizCode, result.bizCode, result.note);
// console.log test-你好 test-你好 213

```
### 核心理念
* dto类多次复用
* 针对不同场景，使用不同的方式进行检验和转化
* 校验和转化都是鸭子类型进行转化
