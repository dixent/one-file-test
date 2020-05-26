 <table class="tfo-notebook-buttons" align="left">
  <td>
    <a target="_blank" href="https://www.tensorflow.org/tutorials/quickstart/beginner">
      <img src="https://www.tensorflow.org/images/tf_logo_32px.png"/>
      Смотрите на TensorFlow.org
    </a>
  </td>
  <td>
    <a target="_blank" href="https://colab.research.google.com/github/tensorflow/docs-l10n/blob/master/site/ru/tutorials/quickstart/beginner.ipynb">
      <img src="https://www.tensorflow.org/images/colab_logo_32px.png"/>
      Запустите в Google Colab
    </a>
  </td>
  <td>
    <a target="_blank" href="https://github.com/tensorflow/docs-l10n/blob/master/site/ru/tutorials/quickstart/beginner.ipynb">
      <img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png"/>
      Изучайте код на GitHub
    </a>
  </td>
  <td>
    <a href="https://storage.googleapis.com/tensorflow_docs/docs-l10n/site/ru/tutorials/quickstart/beginner.ipynb">
      <img src="https://www.tensorflow.org/images/download_logo_32px.png"/>
      Скачайте ноутбук
    </a>
  </td>
</table>

TF 2.0 объединяет в себе простоту eager execution и мощь TF 1.0. В центре этого слияния находится `tf.function`, позволяющий преобразовывать подмножество синтаксиса Python в переносимые высокопроизводительные графы TensorFlow.
AutoGraph - крутая новая функция `tf.function`, которая позволяет писать код графа с использованием естественного синтаксиса Python. Список возможностей Python, которые можно использовать с AutoGraph, см. в [Возможности и ограничения AutoGraph](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/autograph/g3doc/reference/limitations) мкр. Для  дополнительной информации о `tf.function` см. RFC
[TF 2.0: функции, а не сессии](https://github.com/tensorflow/community/blob/master/rfcs/20180918-functions-not-sessions-20.md). Для более подробной информации об AutoGraph, см. `tf.autograph`.
Этот учебник познакомит вас с базовыми функциями `tf.function` и AutoGraph.
