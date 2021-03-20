## Binary format
Jackson supports a new json compatible binary format [smile](https://github.com/FasterXML/smile-format-specification/blob/master/smile-specification.md)

## Imrpove perfomance
Add **Blackbird** as a module to improve reflection perfomance. See [Blackbird](https://github.com/FasterXML/jackson-modules-base/tree/master/blackbird)

```
ObjectMapper mapper = JsonMapper.builder()
    .addModule(new BlackbirdModule())
    .build();
```
