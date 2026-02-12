# 🌾 e-Mandi 

An all-in-one digital marketplace that connects farmers with buyers for the sale and purchase of agricultural produce — featuring real-time listings, order tracking, and category-based crop discovery.

![License](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/status-Active-brightgreen)
![Made With](https://img.shields.io/badge/Made%20with-Node.js-339933?logo=node.js&logoColor=white)

---

## ✨ Features

### 👨‍🌾 Farmers
- Register/Login
- List crops with full details
- Edit/Delete their listings
- Track orders in real-time

### 🛒 Buyers
- Browse/search crops by category
- View farmer listings with location, quantity, price
- Place orders and track delivery

### ⚙️ Admin (if included)
- Approve farmer listings
- Manage users and orders

---

## 💻 Tech Stack

- **Frontend**: React.js / HTML / CSS / Bootstrap / Tailwind
- **Backend**: Node.js + Express.js
- **Database**: MongoDB
- **Authentication**: JWT + bcrypt
- **Hosting**: (e.g., Render, Vercel, Railway, MongoDB Atlas)

---

## 🔧 Installation

```bash
# Clone the repository
git clone https://github.com/Arnab-cell/e-Mandi
cd e-mandi

# Install backend dependencies
cd backend
npm install

# Setup environment variables
touch .env
# Add your MONGO_URI, JWT_SECRET, etc.

# Run backend server
npm start

# In a new terminal tab, run frontend
cd ../frontend
npm install
npm start
🌱 Environment Variables
In your backend/.env file:

ini
Copy
Edit
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key
🛡️ License
This project is licensed under the MIT License - see the LICENSE file for details.

🙌 Contributing
Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

📞 Contact
Made by Arnab Chakraborty
📧 Email: arnabchakraborty.sam@gmail.com
🌐 Portfolio: https://arnab-cell.github.io/Portfolio/
