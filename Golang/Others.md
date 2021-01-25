## Methods
```
func name(var1,var2) (return1,return2)
```

## Matrix
```
	matrix := [][]bool{}
	for _, line := range lines {
		matrixLine := []bool{}
		for _, r := range line {
			if r == '#' {
				matrixLine = append(matrixLine, true)
			} else {
				matrixLine = append(matrixLine, false)
			}
		}
		matrix = append(matrix, matrixLine)
	}
```

## Const Block
```
import (
	"fmt"
	almas "rsc.io/quote"
)

const (
	name    = "Almasj"
	surname = "Abdrazak"
)
```

## Map contains
```
if val, ok := dict["foo"]; ok {
    //do something here
}
```
