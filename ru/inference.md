# Вывод TensorFlow Lite

Термин « *логический вывод»* относится к процессу выполнения модели TensorFlow Lite на устройстве, чтобы делать прогнозы на основе входных данных. Чтобы выполнить логический вывод с помощью модели TensorFlow Lite, вы должны запустить его через *интерпретатор* . Интерпретатор TensorFlow Lite разработан, чтобы быть компактным и быстрым. Интерпретатор использует статический порядок графов и настраиваемый (менее динамичный) распределитель памяти для обеспечения минимальной нагрузки, инициализации и задержки выполнения.

На этой странице описывается, как получить доступ к интерпретатору TensorFlow Lite и выполнить логический вывод с использованием C ++, Java и Python, а также ссылки на другие ресурсы для каждой [поддерживаемой платформы](#supported-platforms) .

[TOC]

## Важные понятия

Вывод TensorFlow Lite обычно состоит из следующих шагов:

1. **Загрузка модели**

    Вы должны загрузить модель `.tflite` в память, которая содержит график выполнения модели.

2. **Преобразование данных**

    Необработанные входные данные для модели обычно не соответствуют формату входных данных, ожидаемому моделью. Например, вам может потребоваться изменить размер изображения или изменить формат изображения, чтобы он был совместим с моделью.

3. **Выполнение вывода**

    Этот шаг включает использование API TensorFlow Lite для выполнения модели. Он включает в себя несколько шагов, таких как создание интерпретатора и размещение тензоров, как описано в следующих разделах.

4. **Устный перевод**

    Когда вы получаете результаты логического вывода модели, вы должны интерпретировать тензоры осмысленным образом, который будет полезен в вашем приложении.

    Например, модель может возвращать только список вероятностей. Вы должны сопоставить вероятности с соответствующими категориями и представить их конечному пользователю.

## Поддерживаемые платформы

API-интерфейсы вывода TensorFlow предоставляются для большинства распространенных мобильных / встроенных платформ, таких как [Android](#android-platform) , [iOS](#ios-platform) и [Linux](#linux-platform) , на нескольких языках программирования.

В большинстве случаев дизайн API отражает предпочтение производительности, а не простоты использования. TensorFlow Lite разработан для быстрого вывода на небольших устройствах, поэтому неудивительно, что API-интерфейсы стараются избегать ненужных копий за счет удобства. Точно так же согласованность с API-интерфейсами TensorFlow не была явной целью, и следует ожидать некоторого расхождения между языками.

Во всех библиотеках API TensorFlow Lite позволяет загружать модели, передавать входные данные и извлекать выходные данные.

### Платформа Android

В Android логический вывод TensorFlow Lite может быть выполнен с использованием API Java или C ++. API Java обеспечивают удобство и могут использоваться непосредственно в классах Android Activity. API C ++ предлагают большую гибкость и скорость, но могут потребовать написания оболочек JNI для перемещения данных между слоями Java и C ++.

See below for details about using [C++](#load-and-run-a-model-in-c) and [Java](#load-and-run-a-model-in-java), or follow the [Android quickstart](android.md) for a tutorial and example code.

#### Генератор кода оболочки Android TensorFlow Lite

Примечание. Генератор кода оболочки TensorFlow Lite находится на экспериментальной (бета) стадии и в настоящее время поддерживает только Android.

Для модели TensorFlow Lite, дополненной [метаданными](../convert/metadata.md) , разработчики могут использовать генератор кода оболочки Android TensorFlow Lite для создания кода оболочки для конкретной платформы. Код оболочки устраняет необходимость прямого взаимодействия с `ByteBuffer` на Android. Вместо этого разработчики могут взаимодействовать с моделью TensorFlow Lite с типизированными объектами, такими как `Bitmap` и `Rect` . Для получения дополнительной информации, пожалуйста, обратитесь к [генератору кода оболочки TensorFlow Lite для Android](../inference_with_metadata/codegen.md) .

### Платформа iOS

В iOS TensorFlow Lite доступен с собственными библиотеками iOS, написанными на [Swift](https://www.tensorflow.org/code/tensorflow/lite/experimental/swift) и [Objective-C](https://www.tensorflow.org/code/tensorflow/lite/experimental/objc) . Вы также можете использовать [C API](https://www.tensorflow.org/code/tensorflow/lite/c/c_api.h) непосредственно в кодах Objective-C.

See below for details about using [Swift](#load-and-run-a-model-in-swift), [Objective-C](#load-and-run-a-model-in-objective-c) and the [C API](#using-c-api-in-objective-c-code), or follow the [iOS quickstart](ios.md) for a tutorial and example code.

### Платформа Linux

На платформах Linux (включая [Raspberry Pi](build_rpi.md) ) вы можете выполнять выводы с помощью API-интерфейсов TensorFlow Lite, доступных на [C ++](#load-and-run-a-model-in-c) и [Python](#load-and-run-a-model-in-python) , как показано в следующих разделах.

## Запуск модели

Запуск модели TensorFlow Lite включает несколько простых шагов:

1. Загрузите модель в память.
2. Создайте `Interpreter` на основе существующей модели.
3. Установите значения входного тензора. (При необходимости измените размер входных тензоров, если предопределенные размеры не нужны.)
4. Вызвать вывод.
5. Считайте выходные значения тензора.

В следующих разделах описывается, как эти шаги могут быть выполнены на каждом языке.

## Загрузить и запустить модель на Java

*Платформа: Android*

API Java для выполнения логического вывода с помощью TensorFlow Lite в первую очередь предназначен для использования с Android, поэтому он доступен как зависимость библиотеки Android: `org.tensorflow:tensorflow-lite` .

В Java вы будете использовать класс `Interpreter` для загрузки модели и вывода модели. Во многих случаях это может быть единственный API, который вам нужен.

Вы можете инициализировать `Interpreter` используя файл `.tflite` :

```java
public Interpreter(@NotNull File modelFile);
```

Или с `MappedByteBuffer` :

```java
public Interpreter(@NotNull MappedByteBuffer mappedByteBuffer);
```

В обоих случаях необходимо предоставить действительную модель TensorFlow Lite, иначе API `IllegalArgumentException` . Если вы используете `MappedByteBuffer` для инициализации `Interpreter` , он должен оставаться неизменным в течение всего времени существования `Interpreter` .

Чтобы затем выполнить логический вывод с моделью, просто вызовите `Interpreter.run()` . Например:

```java
try (Interpreter interpreter = new Interpreter(file_of_a_tensorflowlite_model)) {
  interpreter.run(input, output);
}
```

Метод `run()` принимает только один ввод и возвращает только один вывод. Поэтому, если ваша модель имеет несколько входов или несколько выходов, вместо этого используйте:

```java
interpreter.runForMultipleInputsOutputs(inputs, map_of_indices_to_outputs);
```

В этом случае каждая запись во `inputs` соответствует входному тензору, а `map_of_indices_to_outputs` отображает индексы выходных тензоров в соответствующие выходные данные.

В обоих случаях тензорные индексы должны соответствовать значениям, которые вы [указали конвертеру TensorFlow Lite](../convert/) при создании модели. Имейте в виду, что порядок тензоров на `input` должен соответствовать порядку, указанному в TensorFlow Lite Converter.

Класс `Interpreter` также предоставляет удобные функции для получения индекса любого ввода или вывода модели с использованием имени операции:

```java
public int getInputIndex(String opName);
public int getOutputIndex(String opName);
```

Если `opName` не является допустимой операцией в модели, `IllegalArgumentException` .

Также помните, что `Interpreter` владеет ресурсами. Чтобы избежать утечки памяти, ресурсы должны быть освобождены после использования:

```java
interpreter.close();
```

Пример проекта с Java см. В [примере классификации изображений Android](https://github.com/tensorflow/examples/tree/master/lite/examples/image_classification/android) .

### Поддерживаемые типы данных (в Java)

Чтобы использовать TensorFlow Lite, типы данных входных и выходных тензоров должны быть одним из следующих примитивных типов:

- `float`
- `int`
- `long`
- `byte`

`String` типы также поддерживаются, но они кодируются иначе, чем примитивные типы. В частности, форма тензора строки определяет количество и расположение строк в тензоре, при этом каждый элемент сам по себе является строкой переменной длины. В этом смысле (байтовый) размер Tensor не может быть вычислен только на основе формы и типа, и, следовательно, строки не могут быть предоставлены как один плоский аргумент `ByteBuffer` .

Если используются другие типы данных, включая упакованные в коробку типы, такие как `Integer` и `Float` , будет создано `IllegalArgumentException` .

#### Входы

Каждый ввод должен быть массивом или многомерным массивом поддерживаемых типов примитивов или необработанным `ByteBuffer` соответствующего размера. Если входные данные являются массивом или многомерным массивом, связанный входной тензор будет неявно изменен до размеров массива во время вывода. Если входом является ByteBuffer, вызывающий должен сначала вручную изменить размер связанного входного тензора (через `Interpreter.resizeInput()` ) перед выполнением логического вывода.

При использовании `ByteBuffer` предпочтительнее использовать прямые байтовые буферы, так как это позволяет `Interpreter` избегать ненужных копий. Если `ByteBuffer` является прямым байтовым буфером, его порядок должен быть `ByteOrder.nativeOrder()` . После использования для вывода модели, он должен оставаться неизменным до завершения вывода модели.

#### Выходы

Каждый вывод должен быть массивом или многомерным массивом поддерживаемых типов примитивов или ByteBuffer соответствующего размера. Обратите внимание, что некоторые модели имеют динамические выходы, где форма выходных тензоров может варьироваться в зависимости от входа. Нет простого способа справиться с этим с помощью существующего API вывода Java, но запланированные расширения сделают это возможным.

## Загрузить и запустить модель в Swift

*Платформа: iOS*

[Swift API](https://www.tensorflow.org/code/tensorflow/lite/experimental/swift) доступен в `TensorFlowLiteSwift` Pod от Cocoapods.

Во-первых, вам нужно импортировать модуль `TensorFlowLite` .

```swift
import TensorFlowLite
```

```swift
// Getting model path
guard
  let modelPath = Bundle.main.path(forResource: "model", ofType: "tflite")
else {
  // Error handling...
}

do {
  // Initialize an interpreter with the model.
  let interpreter = try Interpreter(modelPath: modelPath)

  // Allocate memory for the model's input `Tensor`s.
  try interpreter.allocateTensors()

  let inputData: Data  // Should be initialized

  // input data preparation...

  // Copy the input data to the input `Tensor`.
  try self.interpreter.copy(inputData, toInputAt: 0)

  // Run inference by invoking the `Interpreter`.
  try self.interpreter.invoke()

  // Get the output `Tensor`
  let outputTensor = try self.interpreter.output(at: 0)

  // Copy output to `Data` to process the inference results.
  let outputSize = outputTensor.shape.dimensions.reduce(1, {x, y in x * y})
  let outputData =
        UnsafeMutableBufferPointer&lt;Float32&gt;.allocate(capacity: outputSize)
  outputTensor.data.copyBytes(to: outputData)

  if (error != nil) { /* Error handling... */ }
} catch error {
  // Error handling...
}
```

## Загрузить и запустить модель в Objective-C

*Платформа: iOS*

[API Objective-C](https://www.tensorflow.org/code/tensorflow/lite/experimental/objc) доступен в `TensorFlowLiteObjC` Pod от Cocoapods.

Во-первых, вам нужно импортировать модуль `TensorFlowLite` .

```objc
@import TensorFlowLite;
```

```objc
NSString *modelPath = [[NSBundle mainBundle] pathForResource:@"model"
                                                      ofType:@"tflite"];
NSError *error;

// Initialize an interpreter with the model.
TFLInterpreter *interpreter = [[TFLInterpreter alloc] initWithModelPath:modelPath
                                                                  error:&amp;error];
if (error != nil) { /* Error handling... */ }

// Allocate memory for the model's input `TFLTensor`s.
[interpreter allocateTensorsWithError:&amp;error];
if (error != nil) { /* Error handling... */ }

NSMutableData *inputData;  // Should be initialized
// input data preparation...

// Copy the input data to the input `TFLTensor`.
[interpreter copyData:inputData toInputTensorAtIndex:0 error:&amp;error];
if (error != nil) { /* Error handling... */ }

// Run inference by invoking the `TFLInterpreter`.
[interpreter invokeWithError:&amp;error];
if (error != nil) { /* Error handling... */ }

// Get the output `TFLTensor`
TFLTensor *outputTensor = [interpreter outputTensorAtIndex:0 error:&amp;error];
if (error != nil) { /* Error handling... */ }

// Copy output to `NSData` to process the inference results.
NSData *outputData = [outputTensor dataWithError:&amp;amp;error];
if (error != nil) { /* Error handling... */ }
```

### Использование C API в коде Objective-C

В настоящее время Objective-C API не поддерживает делегатов. Чтобы использовать делегаты с кодом Objective-C, вам необходимо напрямую вызвать базовый [API C.](https://www.tensorflow.org/code/tensorflow/lite/c/c_api.h)

```c
# include "tensorflow/lite/c/c_api.h"

```

```c
TfLiteModel* model = TfLiteModelCreateFromFile([modelPath UTF8String]);
TfLiteInterpreterOptions* options = TfLiteInterpreterOptionsCreate();

// Create the interpreter.
TfLiteInterpreter* interpreter = TfLiteInterpreterCreate(model, options);

// Allocate tensors and populate the input tensor data.
TfLiteInterpreterAllocateTensors(interpreter);
TfLiteTensor* input_tensor =
    TfLiteInterpreterGetInputTensor(interpreter, 0);
TfLiteTensorCopyFromBuffer(input_tensor, input.data(),
                           input.size() * sizeof(float));

// Execute inference.
TfLiteInterpreterInvoke(interpreter);

// Extract the output tensor data.
const TfLiteTensor* output_tensor =
    TfLiteInterpreterGetOutputTensor(interpreter, 0);
TfLiteTensorCopyToBuffer(output_tensor, output.data(),
                         output.size() * sizeof(float));

// Dispose of the model and interpreter objects.
TfLiteInterpreterDelete(interpreter);
TfLiteInterpreterOptionsDelete(options);
TfLiteModelDelete(model);
```

## Загрузить и запустить модель на C ++

*Платформы: Android, iOS и Linux.*

Примечание. C ++ API на iOS доступен только при использовании bazel.

В C ++ модель хранится в классе [`FlatBufferModel`](https://www.tensorflow.org/lite/api_docs/cc/class/tflite/flat-buffer-model.html) . Он инкапсулирует модель TensorFlow Lite, и вы можете построить ее несколькими способами, в зависимости от того, где хранится модель:

```c++
class FlatBufferModel {
  // Build a model based on a file. Return a nullptr in case of failure.
  static std::unique_ptr&lt;FlatBufferModel&gt; BuildFromFile(
      const char* filename,
      ErrorReporter* error_reporter);

  // Build a model based on a pre-loaded flatbuffer. The caller retains
  // ownership of the buffer and should keep it alive until the returned object
  // is destroyed. Return a nullptr in case of failure.
  static std::unique_ptr&lt;FlatBufferModel&gt; BuildFromBuffer(
      const char* buffer,
      size_t buffer_size,
      ErrorReporter* error_reporter);
};
```

Примечание. Если TensorFlow Lite обнаруживает наличие [Android NNAPI](https://developer.android.com/ndk/guides/neuralnetworks) , он автоматически пытается использовать общую память для хранения `FlatBufferModel` .

Теперь, когда у вас есть модель как объект `FlatBufferModel` , вы можете выполнить ее с помощью [`Interpreter`](https://www.tensorflow.org/lite/api_docs/cc/class/tflite/interpreter.html) . Один `FlatBufferModel` может использоваться одновременно более чем одним `Interpreter` .

Caution: The `FlatBufferModel` object must remain valid until all instances of `Interpreter` using it have been destroyed.

Важные части `Interpreter` API показаны во фрагменте кода ниже. Необходимо отметить, что:

- Тензоры представлены целыми числами, чтобы избежать сравнения строк (и любой фиксированной зависимости от строковых библиотек).
- К интерпретатору нельзя обращаться из параллельных потоков.
- Выделение памяти для тензоров ввода и вывода должно запускаться вызовом `AllocateTensors()` сразу после изменения размера тензоров.

Простейшее использование TensorFlow Lite с C ++ выглядит так:

```c++
// Load the model
std::unique_ptr&lt;tflite::FlatBufferModel&gt; model =
    tflite::FlatBufferModel::BuildFromFile(filename);

// Build the interpreter
tflite::ops::builtin::BuiltinOpResolver resolver;
std::unique_ptr&lt;tflite::Interpreter&gt; interpreter;
tflite::InterpreterBuilder(*model, resolver)(&amp;interpreter);

// Resize input tensors, if desired.
interpreter-&gt;AllocateTensors();

float* input = interpreter-&gt;typed_input_tensor&lt;float&gt;(0);
// Fill `input`.

interpreter-&gt;Invoke();

float* output = interpreter-&gt;typed_output_tensor&lt;float&gt;(0);
```

Для получения дополнительных примеров кода см. [`minimal.cc`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/examples/minimal/minimal.cc) и [`label_image.cc`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/examples/label_image/label_image.cc) .

## Загрузить и запустить модель в Python

*Платформа: Linux*

API Python для выполнения вывода предоставляется в модуле `tf.lite` . Из которого вам в основном нужен только [`tf.lite.Interpreter`](https://www.tensorflow.org/api_docs/python/tf/lite/Interpreter) для загрузки модели и выполнения логического вывода.

В следующем примере показано, как использовать интерпретатор Python для загрузки файла `.tflite` и выполнения логического вывода со случайными входными данными:

```python
import numpy as np
import tensorflow as tf

# Load the TFLite model and allocate tensors.

interpreter = tf.lite.Interpreter(model_path="converted_model.tflite")
interpreter.allocate_tensors()

# Get input and output tensors.

input_details = interpreter.get_input_details()
output_details = interpreter.get_output_details()

# Test the model on random input data.

input_shape = input_details[0]['shape']
input_data = np.array(np.random.random_sample(input_shape), dtype=np.float32)
interpreter.set_tensor(input_details[0]['index'], input_data)

interpreter.invoke()

# The function `get_tensor()` returns a copy of the tensor data.

# Use `tensor()` in order to get a pointer to the tensor.

output_data = interpreter.get_tensor(output_details[0]['index'])
print(output_data)
```

As an alternative to loading the model as a pre-converted `.tflite` file, you can combine your code with the [TensorFlow Lite Converter Python API](https://www.tensorflow.org/lite/convert/python_api) (`tf.lite.TFLiteConverter`), allowing you to convert your TensorFlow model into the TensorFlow Lite format and then run inference:

```python
import numpy as np
import tensorflow as tf

img = tf.placeholder(name="img", dtype=tf.float32, shape=(1, 64, 64, 3))
const = tf.constant([1., 2., 3.]) + tf.constant([1., 4., 4.])
val = img + const
out = tf.identity(val, name="out")

# Convert to TF Lite format

with tf.Session() as sess:
  converter = tf.lite.TFLiteConverter.from_session(sess, [img], [out])
  tflite_model = converter.convert()

# Load the TFLite model and allocate tensors.

interpreter = tf.lite.Interpreter(model_content=tflite_model)
interpreter.allocate_tensors()

# Continue to get tensors and so forth, as shown above...

```

Дополнительные образцы кода Python см. В [`label_image.py`](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/examples/python/label_image.py) .

Совет: запустите `help(tf.lite.Interpreter)` в терминале Python, чтобы получить подробную документацию по интерпретатору.

## Поддерживаемые операции

TensorFlow Lite поддерживает подмножество операций TensorFlow с некоторыми ограничениями. Полный список операций и ограничений см. На [странице TF Lite Ops](https://www.tensorflow.org/mlir/tfl_ops) .
