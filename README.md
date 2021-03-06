# evuv-exp-parser
evuv parser support parsing simple boolean expressions.
Supported operators:
- and
- or
- not
- \> (bigger)
- < (smaller)
- = (equals)
- contains
- exists

evuv parser also support parsing json expression formatted as a json Druid (druid.io) filter expression.


## usage examples (expression builder)
```java
ComparableExpression<GenericNumber> left = ExpressionBuilder.prop("A", GenericNumber.class);
ComparableExpression<GenericNumber> right = ExpressionBuilder.value(new GenericNumber(10.0));
Expression<Boolean> sm = ExpressionBuilder.smaller(left, right);
Expression<Boolean> enot = ExpressionBuilder.not(sm);
Map<String, Object> bindings = new HashMap<>();
bindings.put("A", 1000);
BindedExpression<Boolean> bindedExp = enot.bind(bindings);
Assert.assertTrue(bindedExp.getValue());
 ``` 
    
## usage examples (druid format json) 
 ```java
  String json = { "filter": {
			      "type": "and",
			      "fields": [
			         {
			            "dimension": "sh",
			            "type": "selector",
			            "value": "some_value"
			         },
				 {
			            "dimension": "dim1",
			            "type": "selector",
			            "op": "exists"
			         },
			         {
			            "op": ">",
			            "measure": "m1",
			            "type": "measure",
			            "value": 10
			         },
			         {
			            "op": "contains",
			            "dimension": "dim2",
			            "type": "selector",
			            "value": "xyz"
			         }
			      ]
			   }
			};
      
      SimpleConditionParser parser = new SimpleConditionParser();
      Map<String,Object> bindings = new HashMap<String,Object>();
      bindings.put("sh", "some_value");
      bindings.put("dim2", "orxyzevi");
      bindings.put("dim1", "some_value");
      bindings.put("m1", 100);
      Expression<Boolean> expr = parser.parseCondition(json,  false);
      BindedExpression<Boolean> bindedExpr = expr.bind(bindings);
      Assert.assertTrue(bindedExpr.getValue());
   ```
   
