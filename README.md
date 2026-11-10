# Ecg_heart_analysis_project_deeplearning

ECGFounder AI

ECG Classification Using Deep Learning

مشروع بحثي لتصنيف إشارات تخطيط القلب الكهربائي (ECG) باستخدام نموذج ECGFounder المدرب مسبقًا، مع تطبيق Fine-Tuning على مجموعة بيانات PTB-XL باستخدام PyTorch.

---

1. Project Overview

يهدف المشروع إلى تحليل إشارات ECG وتصنيفها إلى خمس فئات قلبية باستخدام تقنية Multi-Label Classification.

- Pretrained Model: ECGFounder
- Deep Learning Framework: PyTorch
- Task: Multi-Label ECG Classification
- Dataset: PTB-XL v1.0.3

2. Dataset

تم استخدام مجموعة بيانات PTB-XL v1.0.3.

Description| Value
Original Records| 21,799
Used Records| 21,388
ECG Leads| 12
Sampling Rate| 500 Hz
Signal Duration| 10 Seconds
Samples per Lead| 5,000

تم استبعاد 411 سجلًا لعدم احتوائها على التسميات المستهدفة.

Data Splitting

Dataset| Folds| Samples
Training| 1–8| 17,084
Validation| 9| 2,146
Testing| 10| 2,158

3. Target Classes

تم اعتماد خمس فئات للتصنيف:

Class| Description
NORM| Normal ECG
MI| Myocardial Infarction
STTC| ST/T Change
CD| Conduction Disturbance
HYP| Hypertrophy

4. Data Preprocessing

تم تنفيذ خطوات معالجة إشارات ECG التالية:

1. ترتيب اشتقاقات ECG الاثني عشر.
2. تطبيق Bandpass Filtering لإزالة الترددات غير المرغوبة.
3. تطبيق Z-Score Normalization على كل تسجيل.
4. تجهيز الإشارات بالشكل التالي:

[12, 5000]

5. Model Architecture

تم استخدام معمارية ECGFounder الأصلية مع تحميل الأوزان المدربة مسبقًا، واستبدال طبقة التصنيف النهائية لتناسب الفئات الخمس.

Training Configuration

Parameter| Value
Fine-Tuning| Partial Fine-Tuning
Trainable Layers| Last Stages + Dense
Optimizer| AdamW
Backbone Learning Rate| 1e-5
Classification Head LR| 5e-4
Weight Decay| 5e-4
Loss Function| BCEWithLogitsLoss
Class Imbalance| pos_weight
Batch Size| 16
Epochs| 5
Gradient Clipping| 1.0
Mixed Precision| AMP

Training Techniques

- Partial Fine-Tuning: تدريب المراحل الأخيرة من النموذج وطبقة التصنيف.
- AdamW: تحسين أوزان النموذج أثناء التدريب.
- BCEWithLogitsLoss: دالة الخسارة للتصنيف متعدد التسميات.
- pos_weight: التعامل مع عدم توازن الفئات.
- ReduceLROnPlateau: تعديل معدل التعلم بناءً على أداء التحقق.
- Early Stopping: إيقاف التدريب عند عدم تحسن الأداء.
- Best Checkpoint: حفظ أفضل أوزان بناءً على Validation Macro AUROC.

6. Threshold Optimization

تم تحديد عتبة تصنيف مستقلة لكل فئة باستخدام بيانات التحقق Validation Dataset، مع استهداف Recall لا يقل عن 95%.

Optimized Thresholds

OPTIMAL_THRESHOLDS = {
    "NORM": 0.3049,
    "MI": 0.1731,
    "STTC": 0.1964,
    "CD": 0.1812,
    "HYP": 0.0790
}

تم تثبيت العتبات قبل إجراء التقييم النهائي على بيانات الاختبار.

7. Final Test Results

تم تقييم النموذج النهائي باستخدام Fold 10.

Class| AUROC| Precision| Recall| F1-Score
NORM| 0.9455| 0.7486| 0.9647| 0.8430
MI| 0.9324| 0.4906| 0.9527| 0.6477
STTC| 0.9341| 0.5561| 0.9328| 0.6968
CD| 0.9326| 0.4596| 0.9415| 0.6177
HYP| 0.8698| 0.1890| 0.9580| 0.3157

Overall Performance

Metric| Score
Validation Macro AUROC| 0.921893
Final Test Macro AUROC| 0.92287

8. Technologies Used

- Python
- PyTorch
- ECGFounder
- PTB-XL
- NumPy
- Pandas
- Scikit-learn
- Jupyter Notebook

9. Project Files

File| Description
"ECGFounder_Final_Model.pth"| النموذج النهائي المدرب
"thresholds.json"| عتبات التصنيف المعتمدة
"Training Notebook"| دفتر المعالجة والتدريب والتقييم

10. Medical Disclaimer

هذا المشروع بحثي وتجريبي، ويقدم تنبؤات احتمالية باستخدام تقنيات التعلم العميق. لا يُعد أداة تشخيص طبي معتمدة، ولا يغني عن تقييم الطبيب المختص.

---

Author

Anas Al-Alawi

Artificial Intelligence | Deep Learning | Medical AI
