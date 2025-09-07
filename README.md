# 🧾 BwETAFv3-33M: Model Card

**Boring’s Experimental Transformer for Autoregression (Flax)**
A 33M parameter autoregressive language model built using a custom Flax pipeline, loads of tuning, and a sprinkle of existential dread.

> *Trained on determination, fueled by suffering, powered by free TPUs.*

---

## 📌 Model Summary

* **Model Name:** BwETAFv3-33M
* **Parameters:** 33,595,392
* **Training Tokens:** 984,743,936
* **Training Time:** 1.15 TPUv2-8 Hours
* **Framework:** Flax / JAX
* **Max Context Length:** 2048 tokens
* **Tokenizer:** Custom-trained BPE tokenizer (`vocab_size = 16,384`)
* **Dataset Used:** Fineweb (For training and validation) and TinyStories (For training only)

---

## 🧪 Hyperparameters

```json
{
  "num_heads": 4,
  "attention_dim": 512,
  "vocab_size": 16384,
  "num_blocks": 8,
  "ff_dim": 1536,
  "attn_chunks": 1,
  "gqa_repeats": 2,
  "dropout_rate": 0.05,
  "max_len": 2048,
  "use_flash_attention": false,
  "emb_init_range": 0.02,
  "dtype": "bfloat16"
}
```

---

## 🛠 Optimizer Settings


```json
{
  "peaklr": 1.8e-3,
  "warmup_percent": 0.03,
  "min_value": 1.8e-4,
  "training_decay": "cosine",
  "weight_decay": 0.1,
  "min_warmup_value": 6e-4,
  "b1": 0.95,
  "b2": 0.98,
  "eps": 1.5e-8,
  "opt_dtype": "bfloat16"
}
```

## 📈 Performance

* **Final Validation Loss:** `3.39`

* **Training-Validation loss Graphs:**
![image/png](https://cdn-uploads.huggingface.co/production/uploads/661e235e08dd378c818654ad/oYiIfb4Bc9St3qnBvGA4w.png)

* For detailed stats, refer to `stats.json` in the model files.

---

## ⚡ Quickstart

```bash
!pip install BwETAF==0.6
# If you are having any troubles with compatablility issues with the model like jax, hf and BwETAF run
!pip install --upgrade jax jaxlib flax tiktoken jax_cuda12_plugin datasets flash-attention-jax
```

```python
from BwETAF.api.predictv2 import KV_caching
import BwETAF
import jax
from BwETAF.tokenizer.main import load

model = BwETAF.load_hf("WICKED4950/BwETAFv3-33M",jax.numpy.bfloat16)
tokenizer = load("Loaded_model")

thing = KV_caching(model, top_p=0.92,temperature=0.8)
prompt = """The night sky was alive with lightning, each flash revealing the jagged cliffs ahead. I gripped the letter tighter, knowing it held the answer to everything. The wind screamed as I took a step closer to the edge"""
input = tokenizer.encode(prompt)
print("The input is",input)
the_thing = thing(jax.numpy.array(input),max_len=128)
print(prompt,end="")
for i in the_thing:
    print(tokenizer.decode(i), end="")
print()


# To get the params or jax struct
params = model.trainable_variables
structure = model.model_struct
```

> ☁️ *Colab support and examples coming soon!*

---
## 🧭 Intended Use

This model is primarily released for research and experimentation,  
but it can also be fine-tuned and adapted for downstream tasks.  
Users are encouraged to evaluate it carefully for their specific use cases.  

---
## 📚 Paper

This model is described in detail in the accompanying paper:  
📄 [BwETAFv3: Efficient Autoregressive Language Modeling with 33M Parameters](https://drive.google.com/file/d/1pn9cNypl_hSthIgumnVfSr7tJ9aXU2Cu/view)

The paper covers:  
- Architecture overview  
- Training methodology  
- Regularization & stability techniques  
- Scaling considerations  
- Evaluation results  
- Contact info

---
## 📬 Contact Me

* 📸 Instagram: [boring.\_.wicked](https://www.instagram.com/boring._.wicked/)
* 💬 Discord: `fused_computation.1` *(if you spot me lurking in any AI-related servers)*

---
