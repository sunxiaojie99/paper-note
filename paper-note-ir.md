## ir-gengerate

**[Transformer Memory as a Differentiable Search Index](https://arxiv.org/abs/2202.06991). *Tay et al.*, Arxiv 2022. [[Video](https://www.youtube.com/watch?v=qlB0TPBQ7YY)] (DSI) google**

能否取代retrieval？生成的文档数量有要求吗？能太多吗

最多到320k文档，32w？

Indexing（input: document tokens, output: identifiers）+Retrieval（input: query, output:  a ranked list of candidate docids）

**a DSI model** 

- d->did: trained to index a corpus of documents
- q->did (train) : optionally fine-tune on an available set of labeled data (queries and labeled documents)
- q->did: used to retrieve relevant documents—all within a single, unified model.

options

1. what d？被编码的内容

   - 🌟 <u>Direct Indexing</u>：first L tokens of a document

   - <u>Set Indexing</u>: de-duplicates repeated terms + removes stopwords + first L tokens of a document

   - <u>Inverted Index</u>: randomly subsample a single contiguous chunk of k tokens

     <img src="paper-note-ir.assets/image-20230224141148967.png" alt="image-20230224141148967" style="zoom:43%;" />

2. what did?

   - <u>Unstructured Atomic Identifiers</u>：assign each an arbitrary (and possibly random) unique integer identifier，生成一次就可以生成出来。在一个词表上执行softmax。（不适配新文档加入？）

   - <u>Naively Structured String Identifiers</u> + beam search： treats unstructured identifiers as tokenizable strings

   - 🌟<u>Semantically Structured Identifiers</u>+ beam search：需要满足（1）the docid should capture some information about the semantics of its associated document；（2） the docid should be structured in a way that the search space is effectively reduced after each decoding step；

     - a fully unsupervised pre-processing（对bert编码的doc embedding 层次聚类） -> a fully end-to-end manner(further)

       <img src="paper-note-ir.assets/image-20230224140720545.png" alt="image-20230224140720545" style="zoom:50%;" />

3. how to index？

   - 🌟Inputs2Target：doc_tokens -> docid.
   - Targets2Inputs：docid -> doc_tokens
   - Bidirectional： trains both Inputs2Targets and Targets2Inputs within the same co-training setup（with prefix token ）
   - Span Corruption

4. how to train

   - two-stage ( indexing + fine-tuning: map queries to docids)
   - 🌟 co-train (multi-task learning)： (e.g., using task prompts to differentiate them).

<img src="paper-note-ir.assets/image-20230224141219686.png" alt="image-20230224141219686" style="zoom:30%;" />