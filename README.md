# TabDec

We sincerely thank all the reviewers for their time and efforts in reviewing our paper. We are encouraged that they found: **a)** Our model (with structure and content decoupled prototype memory mechanism) is reasonably well structured and well motivated (R1); **b)** the model achieves competitive results on standard benchmarks (R3); **c)** the Table2Markup dataset fills a genuine gap and offers a highly practical resource (R2,R3). 

We greatly appreciate that the reviewers’valuable comments and suggestions are helpful to improve the quality of our paper, e.g., providing more comparisons and more details (R1,R2,R3). We will update our paper accordingly. Below are our responses to the comments.

### Reviewer #1
**Q1:** The proposed method is largely a combination of existing components:
Prototype memory / key-value memory → widely used in vision and NLP.
Slot Attention → existing clustering mechanism.
Row/column positional encoding → straightforward extension of 2D positional encoding.

**A:** The proposed structure and content decoupled prototype memory aggregates information from the training data. This is clearly different from memory mechanisms in vision and NLP, which are used to record the context of the current input. The introduction of Slot Attention as a long-range memory to periodically update the memory module is also a new design.


**Q2:** The “decoupled prototype memory” formulation is not fundamentally new, and the probabilistic interpretation reduces to standard attention mechanisms.

**A:** Standard attention is computed from a single input, whereas our decoupled prototype memory consists of a set of prototypes induced from the training corpus. Different memory slots compete during training and then specialize automatically to represent different table layout patterns. The two are different in both concept and implementation. We will make this point clear in the updated paper.


**Q3:** EDD is used as the main baseline, but more recent models (e.g., Donut-style models, newer VLM-based approaches) are not rigorously compared.
Mentioned models (Qwen3-VL, DeepSeek-OCR) are included, but evaluation is unclear and inconsistent.
Comparison with state-of-the-art document VLMs under controlled settings.
Evaluation on real-world datasets beyond PubTabNet.

**A:** Our baseline model is not EDD but GOT (2024), which already outperforms the Donut-style baselines. Thank you for the suggestion. We will further conduct fine-tuning experiments of Qwen3-VL and DeepSeek-OCR on Tab2Markup, and add comparisons with more document VLMs. Further evaluations and more details will be provided as much as possible in the updated paper.


**Q4:** The proposed Table2Markup dataset is largely synthetic (LaTeX-rendered + augmentations). Limited evidence that it improves real-world generalization. The “realistic” augmentations are standard image transformations, not true real-world distributions.

**A:** We are adding generalization experiments on the real-world dataset OmniDocBench. The accurate annotations of the synthetic dataset are a clear advantage.


**Q5:** Only 200 samples checked, which is insufficient for a dataset of this scale.

**A:** Thank you for the suggestion. We have checked the entire dataset, and no errors were found.


**Q6:** Training on Table2Markup vs existing datasets improves performance on external benchmarks.

**A:** We are currently conducting additional tests on the table subset of the real-world document dataset OmniDocBench. Since no other public real-world table dataset provides table-to-markup annotations, we will use public document datasets with table samples and a distribution similar to OmniDocBench for comparison. The results will be updated in the revised paper.


**Q7:** The paper repeatedly claims better modeling of table structure, but TPE is a simple additive embedding, not a structured constraint. No explicit enforcement of grid consistency or logical constraints. Errors shown in qualitative results (missed lines, incorrect segmentation) contradict strong claims.

**A:** Thank you for the constructive suggestion. We will explore this further by introducing explicit enforcement of grid consistency or logical constraints to improve model performance and robustness in more special cases. Our model already performs well on more complex cases, and the examples shown are only used to discuss special situations.


**Q8:** How memory interacts with decoder during generation.

**A:** The output of the memory module and the visual encoder features are concatenated and then fed into the decoder.


**Q9:** Computational overhead of memory modules.

**A:** Depending on the number of memory slots, when we set the slot number to 512, the total parameter size of the memory modules across the three scales is 15M. 


**Q10:** The architecture description is hard to follow, especially the interaction between modules.

**A:** The outputs from the visual encoder at three scales are first combined with table-aware positional encodings and then fed into the memory module, which contains slot attention and a prototype memory bank. The retrieved prototype memory is concatenated with the original encoder features and then passed to the language model for decoding. Slot Attention periodically updates the memory bank during training. We will make this point clear in the updated paper.


**Q11:** Heavy reliance on PubTabNet (synthetic/clean) and self-constructed dataset.
No evaluation on real scanned documents and noisy industrial datasets.

**A:** We are conducting experiments on the table subset of OmniDocBench, which includes the real scanned and noisy industrial documents you mentioned.


**Q12:** Inference speed and memory cost.

**A:** The inference speed is 0.1 FPS. The model has 0.6B parameters, compared with 0.58B for the baseline model. We will update these details in the paper.


**Q13:** Limited qualitative comparisons with competing methods.

**A:** Thank you for the suggestion. We will consider adding qualitative comparisons with other models.


### Reviewer #2
**Q1:** The limited novelty of individual components. Slot Attention (Locatello et al., 2020) and prototype memory networks are well-established. The table-aware positional encoding (TPE) is essentially learned row/column embeddings added to features, which is a straightforward extension. The novelty lies more in the combination than in any single component.

**A:** Please refer to Reviewer 1, Q1.


**Q2:** Severely outdated baselines on PubTabNet. The paper only compares against EDD (ECCV 2020) and traditional tools (Tabula, Camelot, etc.). TabDec's overall TEDS of 91.1% is actually below the 2022 result of TableFormer (CVPR 2022) (~93.6%). Missing comparisons with more recent works, such as OmniParser, VAST, UniTable, and other recent methods (2023–2025), make the claimed state-of-the-art improvement unconvincing.

**A:** We selected GOT (2024) as our baseline and compared our method with it. Thank you for the reminder, we will add a comparison with TableFormer in the paper. Although TableFormer reports better results on PubTabNet, we note that it was trained on PubTabNet (568k) for more than 24 epochs. Due to the lack of markup annotations for most PubTabNet images and our limited computation resources, we trained on only a 9k subset for about 10 epochs and still achieved a comparable recognition level. This shows that TabDec remains competitive. For recent open-source methods, we will add more comparison experiments.


**Q3:** Small and domain-limited dataset. Table2Markup contains only ~31K images from arXiv papers (cs, physics, math), which is much smaller than PubTabNet's 568K images. The evaluation on Table2Markup is essentially a self-benchmark with limited external validation. The domain diversity (only academic LaTeX tables) raises concerns about generalization claims.

**A:** Table2Markup provides richer, more detailed, and more accurate annotations than PubTabNet. It not only includes the annotations used in traditional TD, TSR, and TCR table-recognition tasks, but also supports the new table-to-markup task. We are currently running experiments on external benchmarks to verify the generalization of the dataset.


**Q4:** Unfair comparison on Table2Markup. Comparing TabDec (fine-tuned on Table2Markup) against Qwen3-VL, DeepSeek-OCR, and GOT (likely zero-shot or not fine-tuned on this dataset) in Tables 4–5 is not a fair comparison. The large performance gap may simply reflect domain adaptation rather than architectural superiority.

**A:** Thank you for the suggestion. We have already added fine-tuning results of GOT on the LaTeX subset of Table2Markup. The TEDS scores on clean rendered images and simulated photographic images are 93.7 and 89.3, respectively, which are lower than our results (97.6 and 95.5). We also recognize the importance of fair comparison. For the remaining VLMs, fine-tuning has not yet been completed due to computational limits, as they are more than five times larger than our model. We will continue these experiments and update the results in the paper.


**Q5:** Missing important experimental details. The paper does not report the number of training epochs, learning rate schedule, total training time, or the number of model parameters.

**A:** After loading the pretrained GOT weights, the model was trained for 80,000 steps with a learning rate of 1e-5, taking about 5 hours. The model has 0.6B parameters. We will update these details in the paper.


### Reviewer #3
**Q1:** Clarify the Source of Performance Gains: Provide a brief discussion or preliminary analysis isolating the impact of the new architecture from the impact of the high-quality Table2Markup training data. It is currently unclear how much of the model's success on complex tables is simply due to better data.

**A:** The ablation and comparison experiments on PubTabNet use only public data, so the gains can be attributed entirely to the new architecture. In addition, following your suggestion, we added results for GOT fine-tuned on the LaTeX subset of Table2Markup. Its TEDS is 93.7 on clean rendered images and 89.3 on simulated photographic images, with the same training setup as TabDec. Under the same training data, TabDec still achieves better performance (97.6 and 95.5).


**Q2:** Address Simpler Structural Baselines: Respond to the concern that the problem could be solved with simpler, standard techniques. If running a comparison against a standard Vision-Language Model fine-tuned with standard 2D positional encodings (like RoPE) is not feasible during the rebuttal, the authors must acknowledge these alternatives in the text and tone down claims regarding the absolute necessity of the decoupled prototype memory.

**A:** We added results for the baseline+RoPE variant on Table2Markup. The training setup is the same as in the ablation study. The TEDS is 96.1 on simple images (baseline: 93.7, TabRec: 97.6) and 94.3 on complex images (baseline: 89.3, TabRec: 95.5). Our model still has an advantage in terms of results, but your suggestion improves the model’s spatial awareness in a very concise and clear way. This is very helpful and inspiring for our work. We will acknowledge it in the updated paper and continue to explore this direction.
