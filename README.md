# My-n8n-workflow
"Your kitchen team sends one Telegram message. Your inventory updates itself"
Salta3 Orders — Restaurant Automation with n8n

An end-to-end automation system for restaurant order management, built with n8n, Google Sheets, and Telegram. It handles order intake, kitchen notifications, order status tracking, and real-time inventory sync — with zero manual spreadsheet editing.

Overview

This project connects a customer-facing order form to a live operations pipeline. When a customer places an order, it's automatically logged, the kitchen is notified instantly on Telegram, and staff can update order status (Done / Cancel) with a single message — which in turn keeps ingredient stock levels accurate in real time.

Workflows
1. Order Intake

Triggered when a customer submits an order form.

On form submission — captures order details (item, customer name, phone number)
Get row (Sheet2) — retrieves the last order number for sequencing
Append row (Sheet1) — logs the new order with a timestamp and unique order number
Update row (Sheet2) — increments the order counter
Send Telegram message — instantly notifies the kitchen of the new order
2. Order Status & Inventory Sync

Triggered by a simple Telegram message (e.g. 5 Done).

Telegram Trigger — listens for staff messages
Code node (Python) — parses the message into Order Number and Status
Switch — routes the order down a Done or Cancel path
Update row (Sheet1) — updates the order's status
Get row (Sheet1) — retrieves full order details, including ingredients used
Parallel branches — one per ingredient (e.g. bread, cheese, burger), each deducting the used quantity from its stock sheet in real time
Tech Stack
n8n — workflow orchestration
Google Sheets — order database & inventory tracking
Telegram Bot API — staff notifications & status updates
Data Structure

Sheet1 — Orders

Date	Item	For	Phone	Status	Order Number

Sheet2 — Order Counter

Last Number

Stock Sheets — Inventory

ID	Item	Number	Unit
Status
✅ Order intake and kitchen notification — working
✅ Order status updates (Done) with live inventory deduction — working
🚧 Order cancellation (Cancel) path — in progress, does not yet restore deducted stock or send a confirmation message
Notes

Column headers in Google Sheets (e.g. Last Number, Order Number) must match exactly, including case, since n8n's Google Sheets node is case-sensitive when matching or mapping columns.
