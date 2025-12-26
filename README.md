# Central School — Complete Classroom Platform (Phase A)

A comprehensive online classroom platform built with React (frontend) and Node.js/Express (backend). Students can enroll, pay, attend lessons, practice in video/chat rooms, and customize their experience.  Teachers manage courses and lessons.  Admins verify payments and attendance.

## Features

✅ **User Management**
- Student & Teacher registration with JWT authentication
- Personal data collection (name, age, level, birthday, nationality, timetable)
- Role-based access control (student, teacher, admin)

✅ **Enrollment & Assessment**
- Student enrollment form with automatic score-based course recommendations
- Assessment task scaffold
- Payment processing with Stripe Checkout

✅ **Payment & Receipt Verification**
- Stripe Checkout integration
- Applicants upload payment receipts after paying
- Teachers/admins verify or reject receipts before activation

✅ **Customization**
- Primary color picker
- Wallpaper upload (custom backgrounds)
- Dark mode toggle
- Language settings (English, Portuguese, Spanish, French)
- Country selection (Angola, Brazil, Portugal, Mozambique, etc.)

✅ **Course Management (Teachers)**
- Create courses
- Add lessons with video links or file uploads
- Manage course materials

✅ **Practice Rooms**
- Real-time chat (Socket.io)
- WebRTC video calling (peer-to-peer)
- Student discussion & collaboration

✅ **Feedback & Support**
- Students submit feedback on lessons
- Support team receives and manages feedback

✅ **Attendance Tracking**
- Teachers mark attendance per lesson
- Attendance records stored and retrievable
- View attendance history

✅ **Internationalization (i18n)**
- English, Portuguese, Spanish, French
- Easy to add more languages

✅ **Progressive Web App (PWA)**
- Install to home screen
- Offline support ready
- Mobile-friendly UI

---

## Quick Start

### Prerequisites
- Node.js 18+
- MongoDB (local or Atlas)
- Git

### Backend Setup

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```

2. **Copy environment file and configure:**
   ```bash
   cp .env.example .env
   ```

3. **Edit `.env` with your settings:**
   ```
   MONGO_URI=mongodb://localhost:27017/central_school
   JWT_SECRET=your_secret_key_here
   PORT=4000
   STRIPE_SECRET=sk_test_your_stripe_key
   STRIPE_WEBHOOK_SECRET=whsec_your_webhook_secret
   UPLOAD_DIR=uploads
   ```

4. **Install dependencies and run:**
   ```bash
   npm install
   npm run dev
   ```

   Backend runs at `http://localhost:4000`

### Frontend Setup

1. **Navigate to frontend directory:**
   ```bash
   cd ../frontend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start development server:**
   ```bash
   npm start
   ```

   Frontend opens at `http://localhost:3000`

---

## Testing the App

### 1. Splash Screen
- Visit `http://localhost:3000`
- See "Hello welcome to Central School"

### 2. Registration
- Click "Register"
- Fill in personal details (name, email, password, age, level, birthday, nationality, country, language, timetable)
- Create account and receive JWT token

### 3. Enrollment
- Click "Enroll" from splash or register
- Fill enrollment form with personal data
- See automatic course recommendation based on assessment score

### 4. Assessment
- Navigate to `/assessment` (optional scaffold)
- Complete a sample task
- Get a score that determines course level (Beginner < 50, Intermediate 50-80, Advanced > 80)

### 5. Payment with Receipt Verification
- On enrollment, navigate to Payment page
- Click "Pay Now (Stripe Checkout)"
- Use test card: `4242 4242 4242 4242` (any future expiry, any CVC)
- Complete payment and redirect to `/pay-success`
- **Upload payment receipt** (image or PDF)
- Receipt status shows as "pending"
- Admin/teacher verifies receipt via admin panel
- After verification, enrollment becomes "active"

### 6. Stripe Webhook (Local Testing)
If you want the Stripe webhook to automatically mark payments as paid:

```bash
# Install Stripe CLI:  https://stripe.com/docs/stripe-cli
stripe login
stripe listen --forward-to localhost:4000/api/pay/webhook
```

Copy the webhook signing secret displayed and add to `.env`:
```
STRIPE_WEBHOOK_SECRET=whsec_... 
```

### 7. Settings & Customization
- Click "Settings" from splash
- Change primary color
- Upload wallpaper image
- Toggle dark mode
- Settings saved to localStorage (can persist to DB later)

### 8. Teacher Dashboard
- Login as teacher (create teacher account via registration with role adjustment)
- Navigate to `/teacher`
- Create a course
- Add lessons with video links or upload video files
- View and manage all lessons

### 9. Practice Rooms
- Visit `/video/<roomId>` for WebRTC video call
- Visit `/chat/<roomId>` for text discussion
- Invite peers to same room ID to connect

### 10. Feedback
- Navigate to `/feedback`
- Submit feedback on lessons
- Teachers/admins receive and can respond

### 11. Attendance
- Teachers navigate to `/attendance` (requires auth)
- Admin/teacher can view attendance records by course and lesson

### 12. Languages
- From splash, select language (English/Portuguese/Spanish/French)
- UI translates (extensible to all pages)

---

## Project Architecture

### Backend (Node.js + Express + MongoDB)

**Routes:**
- `/api/auth/register` — Student/Teacher registration
- `/api/auth/login` — Login & JWT token generation
- `/api/enroll` — Create enrollment record
- `/api/pay/create-checkout-session` — Stripe Checkout session
- `/api/pay/receipt-upload` — Upload payment receipt
- `/api/pay/receipts/pending` — List pending receipts (admin/teacher)
- `/api/pay/receipt/: enrollmentId/verify` — Verify/reject receipt
- `/api/courses` — CRUD courses (teacher/admin)
- `/api/courses/:id/lessons` — Add lessons to courses
- `/api/uploads/file` — Upload files (videos, images, receipts)
- `/api/feedback` — Submit feedback
- `/api/attendance` — Mark and retrieve attendance

**Models:**
- `User` — Students, teachers, admins
- `Course` — Course metadata + lessons
- `Enrollment` — Student-course enrollment + payment + receipt tracking
- `Feedback` — Student feedback submissions
- `Attendance` — Lesson attendance records

**Real-time:**
- Socket.io for chat rooms and WebRTC signaling

### Frontend (React + i18next + PWA)

**Pages:**
- `/` — Splash screen
- `/register` — Account creation
- `/enroll` — Enrollment form
- `/assessment` — Assessment task
- `/pay` — Payment page
- `/pay-success` — Receipt upload after payment
- `/settings` — Customization (color, wallpaper, dark mode, language)
- `/teacher` — Teacher course & lesson management
- `/video/:roomId` — WebRTC video call room
- `/chat/:id` — Text discussion room
- `/feedback` — Feedback form
- `/attendance` — Attendance viewing

**Internationalization:**
- 4 languages:  English, Portuguese, Spanish, French
- Add more by extending `src/i18n. js`

---

## Deployment

### Backend Deployment

**Render.com (recommended):**
1. Push code to GitHub
2. Connect repo to Render
3. Set environment variables (MONGO_URI, STRIPE_SECRET, etc.)
4. Deploy

**Heroku:**
```bash
heroku create central-school-api
git push heroku main
heroku config:set MONGO_URI=...  JWT_SECRET=...
```

### Frontend Deployment

**Vercel (recommended):**
1. Push code to GitHub
2. Connect frontend folder to Vercel
3. Set REACT_APP_API_URL=https://your-backend-url.com
4. Deploy

**Netlify:**
```bash
npm run build
netlify deploy --prod --dir=build
```

---

## Next Steps / Roadmap

### Phase B (In Progress)
- [ ] Admin receipt verification UI
- [ ] Email notifications & reminders (SMTP)
- [ ] Persistent user settings (save to DB)
- [ ] Profile edit page
- [ ] Course enrollment limits & capacity

### Phase C
- [ ] Multicaixa Express / PayPay AO payment integration (needs API credentials)
- [ ] Zoom / Google Meet integration (programmatic meeting creation)
- [ ] TURN server setup for WebRTC reliability
- [ ] Attendance CSV export
- [ ] Email confirmations & password reset flows

### Phase D
- [ ] Mobile app (React Native)
- [ ] Desktop app (Electron)
- [ ] Advanced analytics (student progress, attendance trends)
- [ ] Certificates upon course completion
- [ ] Peer-to-peer tutoring marketplace

---

## Contributing

1. Clone the repo
2. Create a feature branch:  `git checkout -b feature/your-feature`
3. Make changes and commit: `git commit -m "Add your feature"`
4. Push to branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## Tech Stack

**Backend:**
- Node.js 18+
- Express. js
- MongoDB + Mongoose
- Socket.io (real-time)
- Stripe (payments)
- JWT (authentication)
- Multer (file uploads)
- Bcryptjs (password hashing)

**Frontend:**
- React 18
- React Router v6
- i18next (internationalization)
- Axios (HTTP client)
- Socket.io Client (real-time)
- WebRTC (video calls)

---

## Troubleshooting

### Backend won't start
- Ensure MongoDB is running:  `mongod` (local) or use MongoDB Atlas
- Check MONGO_URI in `.env`
- Check port 4000 is not in use

### Frontend won't start
- Clear node_modules:  `rm -rf node_modules && npm install`
- Clear cache: `npm cache clean --force`

### Stripe Checkout not working
- Ensure STRIPE_SECRET is set in `.env`
- Use test keys (sk_test_...)
- Test card: 4242 4242 4242 4242

### WebRTC video not connecting
- Both peers must join same room ID
- Click "Start Call / Create Offer" to initiate connection
- For production, add TURN servers (currently uses Google STUN)

---

## License

MIT — Feel free to use and modify for your purposes. 

---

## Support

For issues, questions, or feature requests, please open an issue on GitHub or contact the development team.

**Happy learning with Central School!** 🎓
