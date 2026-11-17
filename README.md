# AI Problem Design: Automated Customer Support Ticket Classification

## 1. Use Case Overview
Customer support teams receive hundreds of unstructured queries daily via email and web forms. Manually reading and routing each ticket to the right department (Billing, Technical Support, Account Management, or General Inquiry) causes delays and inconsistent response times. 

This narrow AI use case focuses on **multi-class text classification** to automatically categorize incoming support tickets so they can be routed immediately to the appropriate team.

## 2. Target User
- **Primary Users:** Customer Support Agents and Support Team Leads at a mid-sized SaaS or e-commerce company.
- **User Need:** Reduce manual triage time, eliminate routing errors, and ensure urgent technical or billing issues reach specialists quickly.

## 3. Data Source
- **Dataset:** A curated small dataset of **1,500 historical customer support tickets** (CSV/JSON format) exported from an internal helpdesk system (or an open benchmark subset such as the Kaggle Customer Support Ticket Dataset).
- **Structure:**
  - **Input:** `ticket_subject` + `ticket_body` (short text, 20–200 words).
  - **Output Label:** One of 4 categories (`Billing`, `Technical_Issue`, `Account_Access`, `General_Inquiry`).
- **Split:** 70% Training (1,050 tickets), 15% Validation (225 tickets), 15% Test (225 tickets).

## 4. Constraints
- **Data Size:** Limited to 1,500 labeled examples, requiring a lightweight pre-trained transformer (e.g., DistilBERT) or TF-IDF + Logistic Regression baseline rather than training a large model from scratch.
- **Latency:** Predictions must complete in **under 300 milliseconds** per ticket to support real-time routing.
- **Privacy & Security:** Personally Identifiable Information (PII) such as emails, phone numbers, and account IDs must be masked before model inference.
- **Compute Budget:** Must run on standard CPU inference without requiring expensive dedicated GPUs.

## 5. Evaluation Approach & Success Criteria
- **Primary Metric:** **Macro F1-Score** across all 4 categories (to ensure minority classes are not ignored).
- **Secondary Metrics:** Overall Accuracy, per-class Precision/Recall, and Confusion Matrix analysis to inspect misclassifications between overlapping categories (e.g., `Billing` vs. `Account_Access`).
- **Success Criteria for Deployment:**
  - Macro F1-Score **≥ 0.85** on the held-out test set.
  - Minimum Recall of **≥ 0.88** on high-priority `Technical_Issue` and `Billing` tickets.
  - P95 inference latency **< 300 ms** on CPU.
  - Low-confidence predictions (confidence score `< 0.70`) automatically flagged for human review.# SWYNEX-AI-Problem-Design
