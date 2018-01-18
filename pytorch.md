# pytorch_notes
Este repositorio unicamente contiene notas personales sobre el uso de Pytorch.

## Visión General:
Pytorch intenta ser tres cosas complementarias:
- módulo **torch**: Una librería de operaciones con tensores y arrays, similar a Numpy, pero, a diferencia de Numpy, permite utilizar GPUs.
- módulo **torch.autograd**: Una libería que permite hacer diferenciación automática sobre operaciones con tensores (definidos con el módulo **torch**)
- módulo **torch.nn**: Con la capacidad de operar con tensores y de hacer diferenciación automática es todo lo que se necesita para programar _redes neuronales_. El módulo **torch.nn** aporta facilidades para ello.

## torch.nn.Module:

### Un pequeño inciso sobre _import statements_.
Pytorch utiliza _import statemens_ en sus definición de módulos. Esto provoca una cierta inconsistencia entre el código fuente y la documentación que puede llevar a confusión.
Por ejemplo (aunque ocurre con más clases), el código fuente de la clase Module, está en la ruta **torch\nn\modules\Module**, pero para utilizarla en nuestro código, el _import_ recomendado es `import torch.nn.Module`

_importante no confundir aquí el concepto de module python (un fichero en el que definimos código) con la clase Module de Pytorch, que en este caso estamos discutiendo._

La razón para esto, es que en el modulo **torch.nn** hay ya un _import statement_ que importa **torch\nn\modules\Module**.
La documentación para **torch\nn\modules\Module** esta en la documentación para el módulo python **torch.nn**. Así que es posible ver la documentación y no encontrar el código donde se supone que esta y viceversa.


### El papel de torch.nn.Module
**torch.nn.modules.Module** es una parte importante de la funcionanlidad de redes neuronales de pytorch y tiene algunas peculiaridades que merece la pena comentar.
**torch.nn.modules.Module** representa el _node_ de un _computational graph_ y, como tal, puede tener padres e hijos.
**torch.nn.modules.Module** no se instanciará directamente en nuestro código, sino que se utilizará normalmente como _base class_: 
Al crear un modelo, nuestro modelo heredará de **torch.nn.Module**.
```python
import torch.nn
class MyModel(nn.Module):
	def __init__(self):
		super(MyModel, self).__init__()
```
Nuestro modelo estará compuesto de diferentes operaciones (no lo llamariamos una _layer_ porque en terminología estandar, una _layer_ es, a menudo el resultado de una _linear operation_ y una _activation operation_). En pytorch estas operaciones están definidas en el módulo **torch.nn** y heredan todas de **torch.nn.modules.Module**.
Todas las clases que heredan de **torch.nn.modules.Module** implementan \_\_call\_\_ Esto significa que todas esas clases, una vez instanciadas, podrán llamarse como si fueran funciones (en lugar de llamar a un método).

