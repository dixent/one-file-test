Note: C++ API on iOS is only available when using bazel.

In C++, the model is stored in
[`FlatBufferModel`](https://www.tensorflow.org/lite/api_docs/cc/class/tflite/flat-buffer-model.html)
class. It encapsulates a TensorFlow Lite model and you can build it in a couple
of different ways, depending on where the model is stored:

```c++
class FlatBufferModel {
  // Build a model based on a file. Return a nullptr in case of failure.
  static std::unique_ptr<FlatBufferModel> BuildFromFile(
      const char* filename,
      ErrorReporter* error_reporter);

  // Build a model based on a pre-loaded flatbuffer. The caller retains
  // ownership of the buffer and should keep it alive until the returned object
  // is destroyed. Return a nullptr in case of failure.
  static std::unique_ptr<FlatBufferModel> BuildFromBuffer(
      const char* buffer,
      size_t buffer_size,
      ErrorReporter* error_reporter);
};
```

Note: If TensorFlow Lite detects the presence of the
[Android NNAPI](https://developer.android.com/ndk/guides/neuralnetworks), it
will automatically try to use shared memory to store the `FlatBufferModel`.
