# Optimization Algorithms — Hand Notes

এই repository তে Gradient Descent থেকে শুরু করে Adam পর্যন্ত বিভিন্ন optimization algorithm এর হাতে লেখা নোট শেয়ার করা হয়েছে। প্রতিটি optimizer এর intuition, math, এবং কোথায় ভালো কাজ করে — সব কিছু সহজ ভাষায় বোঝানোর চেষ্টা করা হয়েছে।

---

## যা যা Cover করা হয়েছে

### 1. Normal Gradient Descent এবং এর সমস্যা

তিন ধরনের GD নিয়ে আলোচনা করা হয়েছে:

- **Batch GD** — পুরো dataset একসাথে দেখে update করে
- **SGD (Stochastic GD)** — একটা একটা sample দেখে update করে
- **Mini-batch GD** — ছোট ছোট batch এ ভাগ করে update করে

**Challenges যেগুলো দেখা যায়:**
- Learning rate ঠিক করা কঠিন
- সব feature এর জন্য একই learning rate — sparse data তে সমস্যা
- Local minima ও saddle point এ আটকে যায়
- Oscillation হয় — সোজা পথে না গিয়ে আঁকাবাঁকা পথে যায়

---

### 2. EWMA — Exponentially Weighted Moving Average

**Full Form:** Exponentially Weighted Moving Average

**Intuition:** Past values এর উপর exponentially কমতে থাকা weight দিয়ে একটা smooth average বের করা।

**Formula:**

$$V_t = \beta \cdot V_{t-1} + (1 - \beta) \cdot \theta_t$$

- `β` বড় হলে → বেশি smooth, কিন্তু slow to adapt
- `β` ছোট হলে → noisy, কিন্তু recent values কে বেশি গুরুত্ব দেয়

---

### 3. SGD with Momentum

**Convex ও Non-convex** উভয় graph এ কীভাবে কাজ করে সেটা দেখা হয়েছে।

**Core Idea:** Gradient এর সাথে আগের velocity যোগ করে update করা — বলের গতির মতো।

**Formula:**

$$v_t = \beta \cdot v_{t-1} + (1 - \beta) \cdot g_t$$
$$\theta = \theta - \alpha \cdot v_t$$

**β এর Effect:**
- β বাড়লে → momentum বেশি, oscillation কমে, কিন্তু overshoot হতে পারে
- β কমলে → normal GD এর কাছাকাছি আচরণ করে

---

### 4. NAG — Nesterov Accelerated Gradient

**Full Form:** Nesterov Accelerated Gradient

Momentum এর উপর একটা ছোট কিন্তু গুরুত্বপূর্ণ upgrade।

**Momentum vs NAG:**
- Momentum: আগের gradient দেখে step নেয়
- NAG: আগে একটু এগিয়ে গিয়ে সেখান থেকে gradient দেখে — ফলে **oscillation কমে** এবং faster convergence হয়

**Disadvantage:**
- Hyperparameter tuning একটু জটিল
- Non-convex surface এ সবসময় ভালো নাও করতে পারে

---

### 5. Adagrad — Adaptive Gradient

**Core Concept:** সব parameter এর জন্য আলাদা আলাদা learning rate।

**Sparse data তে কেন ভালো কাজ করে:**
কম দেখা যাওয়া features এর জন্য বড় learning rate, বেশি দেখা যাওয়া features এর জন্য ছোট learning rate — automatically।

**সমস্যা:**
সময়ের সাথে সাথে accumulated gradient (Gt) অনেক বড় হয়ে যায় → learning rate প্রায় শূন্য হয়ে যায় → training থেমে যায়।

---

### 6. RMSProp — Root Mean Square Propagation

Adagrad এর **learning rate শূন্য হয়ে যাওয়ার সমস্যা** সমাধান করে।

**Elongated graph এর সমস্যা:**
Adagrad এ `V_t` ক্রমাগত বাড়তে থাকে, তাই elongated gradient landscape এ learning rate অনেক ছোট হয়ে যায়।

**RMSProp কীভাবে ঠিক করে:**
EWMA ব্যবহার করে `V_t` calculate করে — পুরোনো gradients ধীরে ধীরে ভুলে যায়, recent gradients বেশি গুরুত্ব পায়।


---

### 7. Adam — Adaptive Moment Estimation

**Momentum + RMSProp** একসাথে — সবচেয়ে widely used optimizer।

**দুটো জিনিস track করে:**
- **1st moment (m_t):** gradient এর mean → Momentum এর মতো
- **2nd moment (v_t):** gradient² এর mean → RMSProp এর মতো

**Bias Correction সহ Formula:**

$$m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t$$
$$v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2$$
$$\hat{m}_t = \frac{m_t}{1-\beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t}$$
$$\theta = \theta - \frac{\alpha}{\sqrt{\hat{v}_t} + \epsilon} \cdot \hat{m}_t$$

**Default values:** β₁ = 0.9, β₂ = 0.999, ε = 1e-8

---

## 📁 Repository Structure

```
📦 optimizers-hand-notes
 ┣ 📄 README.md          ← এই ফাইল
 ┗ 📄 optimizers.pdf     ← হাতে লেখা নোট (২০ পেজ)
```

---

> 🖊️ *এই নোটগুলো সম্পূর্ণ হাতে লেখা — নিজে বোঝার এবং অন্যদের সাথে শেয়ার করার উদ্দেশ্যে তৈরি।*
