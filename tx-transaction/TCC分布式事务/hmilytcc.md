# hmily tcc transaction 的入门

## 1. 在接口上添加@Hmily注解（dobbo需要添加在api接口上，springcloud则需要加到feignClient上）

## 2. 在实现接口上添加@Hmily(confirmMethod="",cancelMethod=""),并提供confirm,cancel的方法名称即可（ps，参与方法、确认方法、取消方法的参数必须一致）

