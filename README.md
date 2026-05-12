# Modul 9 — Visualizing and Architectural Risk

**Group:** B04 - MySawit

---

## Deliverable G.1 — Current Architecture

### Context Diagram (Current)

![Context Diagram](./Context_Diagram.png)

---

### Container Diagram (Current)

![Container Diagram](./Container_Diagram.png)

---

### Deployment Diagram (Current)

![Deployment Diagram](./Deployment_Diagram.png)

---

## Deliverable G.2 — Future Architecture (Risk-Stormed)


### Context Diagram (Future)

<!-- TODO: Replace with future context diagram after risk storming -->
![Future Context Diagram](./Future_Context_Diagram.png)

---

### Container Diagram (Future)

<!-- TODO: Replace with future container diagram after risk storming -->
![Future Container Diagram](./Future_Container_Diagram.png)

---

## Deliverable G.3 — Risk Storming Justification


<!-- TODO: Write 2-3 paragraphs explaining:
  1. What risks were identified in the current architecture
  2. How the Risk Storming technique was applied
  3. Why the architectural modifications in G.2 mitigate those risks -->

### Risk Analysis and Architecture Modification Justification

Dengan menerapkan teknik Risk Storming dan membayangkan MySawit sebagai sistem enterprise yang sangat sukses dalam menangani ribuan panen dan pengiriman harian, beberapa risiko arsitektural yang kritis telah teridentifikasi. Risiko yang paling menonjol terletak pada penanganan event sinkron (synchronous event handling) saat ini (menggunakan @EventListener internal dari Spring). Saat ini, ketika seorang Mandor menyetujui panen (PanenApprovedEvent), sistem secara sinkron menghitung dan membuat payroll ledgers serta mencoba mengirimkan notifikasi. Pada skala yang masif, jika layanan notifikasi atau penghitungan pembayaran mengalami kendala (hang), seluruh transaksi database akan di-rollback, sehingga mencegah Mandor menyetujui panen tersebut.

Lebih jauh lagi, ketiadaan algoritma optimasi Knapsack untuk modul Pengiriman (Delivery) menyebabkan truk dimuat secara tidak efisien, yang berujung pada kerugian finansial yang masif dalam skala besar. Terakhir, modul Notification saat ini berupa interface yang belum selesai, yang berarti lonjakan tiba-tiba dalam penggunaan sistem akan membuat pengguna tidak dapat melihat pembaruan-pembaruan yang krusial.

### Architecture Modification Justification:
Untuk memitigasi risiko-risiko ini, arsitektur masa depan akan memperkenalkan Message Broker (Kafka/RabbitMQ) untuk memisahkan (decouple) domain events. Ketika sebuah panen disetujui, sistem inti (core system) hanya akan menulis ke database dan mempublikasikan sebuah event ke broker, lalu segera mengembalikan respons sukses kepada pengguna. Worker independen (seperti Notification Worker dan Payroll Worker yang didedikasikan khusus) akan mengonsumsi events tersebut secara asinkron (asynchronously), memastikan bahwa kegagalan pada notifikasi tidak akan memblokir operasi perkebunan inti.

Selain itu, kami mengusulkan penambahan Logistics Optimization Engine sebagai layanan atau modul khusus untuk mengimplementasikan algoritma Knapsack, yang secara otomatis merekomendasikan bundel panen 400kg yang paling optimal untuk para pengemudi, sehingga dapat memaksimalkan efisiensi armada dan mengurangi biaya operasional.



---

## Deliverable Individual — Component & Code Diagram

---

### Ammar Muhammad Rafif (2406495602) — Auth & User Management

**Focus:** Google OAuth integration, role-based access control (RBAC), and admin user operations.

#### Component Diagram

![Component Diagram - Auth & User Management](./component_auth.png)

#### Code Diagram

![Code Diagram - Auth & User Management](./code_auth.png)

---

### Davin Fauzan Akmalianto (2406409504) — Manajemen Hasil Panen Sawit

**Focus:** Harvest recording, daily limits, Mandor approval workflows, and async triggers for payroll.

#### Component Diagram

![Component Diagram - Manajemen Hasil Panen](./component_panen.png)

#### Code Diagram

![Code Diagram - Manajemen Hasil Panen](./code_panen.png)

---

### Kadek Ngurah Septyawan Chandra Diputra (2406420772) — Manajemen Pengiriman Hasil Panen Sawit

**Focus:** Delivery assignments, capacity constraints (400kg), state transitions, and multi-tier approval workflows.

#### Component Diagram

![Component Diagram - Manajemen Pengiriman](./component_pengiriman.png)

#### Code Diagram

![Code Diagram - Manajemen Pengiriman](./code_pengiriman.png)

---

### Vincent Valentino Oei (2406353225) — Manajemen Pembayaran

**Focus:** Wage configuration, payroll entity management, balance calculations, and payment gateway integration.

#### Component Diagram

![Component Diagram - Manajemen Pembayaran](./component_pembayaran.png)

#### Code Diagram

![Code Diagram - Manajemen Pembayaran](./code_pembayaran.png)

---

### Yafi Alifuddin (2406437155) — Manajemen Kebun Sawit

**Focus:** Plantation CRUD operations, spatial coordinate validation, and 1-to-1 Mandor assignments.

#### Component Diagram

![Component Diagram - Manajemen Kebun](./component_kebun.png)

#### Code Diagram

![Code Diagram - Manajemen Kebun](./code_kebun.png)

---

