# Hamada TV - Live Football Streaming App

A complete mobile-optimized football live streaming application built with React, Express, and Firebase. Features real-time match schedules, live channels with M3U8 streaming, video player, and secure admin panel.

## Overview

Hamada TV is a comprehensive football streaming platform that allows users to watch live matches and sports channels on mobile devices. The application includes:

- **Home Page**: Live and upcoming match schedules with search functionality
- **Channels Page**: Browse and watch live sports channels
- **Video Player**: Full-featured M3U8 stream player with controls
- **Admin Panel**: Secure interface for managing matches, channels, and ads
- **Static Pages**: About, Contact, and Privacy Policy pages

## Tech Stack

### Frontend
- **React 18** with TypeScript
- **Wouter** for routing
- **Tailwind CSS** for styling
- **Shadcn UI** components
- **HLS.js** for M3U8 video streaming
- **TanStack Query** for data fetching
- **Firebase SDK** for authentication and Firestore

### Backend
- **Express.js** server
- **Firebase Admin SDK** (if needed for server-side operations)
- **Vite** for development and bundling

### Database & Storage
- **Firebase Firestore** for real-time data storage
- **Firebase Authentication** for admin security
- **Firebase Storage** for file uploads (logos, images)

## Project Structure

```
├── client/
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   │   ├── BottomNav.tsx  # Mobile navigation
│   │   │   ├── Header.tsx     # App header with theme toggle
│   │   │   ├── MatchCard.tsx  # Match display card
│   │   │   ├── ChannelCard.tsx # Channel display card
│   │   │   ├── VideoPlayer.tsx # M3U8 video player
│   │   │   └── AdPlaceholder.tsx # Ad slots
│   │   ├── pages/             # App pages
│   │   │   ├── Home.tsx       # Match schedule
│   │   │   ├── Channels.tsx   # Live channels
│   │   │   ├── Watch.tsx      # Video player page
│   │   │   ├── Admin.tsx      # Admin panel
│   │   │   ├── About.tsx      # About page
│   │   │   ├── Contact.tsx    # Contact page
│   │   │   └── Privacy.tsx    # Privacy policy
│   │   ├── lib/
│   │   │   ├── firebase.ts    # Firebase configuration
│   │   │   ├── firebaseService.ts # Firestore operations
│   │   │   └── queryClient.ts # React Query setup
│   │   └── App.tsx            # Main app component
│   └── index.html
├── server/
│   ├── index.ts               # Express server
│   └── routes.ts              # API routes (currently minimal)
├── shared/
│   └── schema.ts              # Shared TypeScript types and Zod schemas
└── design_guidelines.md       # Design system documentation
```

## Firebase Collections

### Matches Collection
```typescript
{
  id: string;
  homeTeam: string;
  awayTeam: string;
  homeTeamLogo?: string;
  awayTeamLogo?: string;
  date: string;
  time: string;
  isLive: boolean;
  streamUrl: string;
  createdAt: string;
}
```

### Channels Collection
```typescript
{
  id: string;
  name: string;
  logo: string;
  streamUrl: string;
  createdAt: string;
}
```

### Ads Collection
```typescript
{
  id: string;
  title: string;
  imageUrl: string;
  targetUrl?: string;
  position: "home_banner" | "video_player" | "channel_list";
  isActive: boolean;
  createdAt: string;
}
```

## Setup Instructions

### 1. Firebase Configuration

The app uses Firebase for authentication, database, and storage. Your Firebase credentials are already configured as environment variables:

- `VITE_FIREBASE_API_KEY`
- `VITE_FIREBASE_AUTH_DOMAIN`
- `VITE_FIREBASE_PROJECT_ID`
- `VITE_FIREBASE_STORAGE_BUCKET`
- `VITE_FIREBASE_APP_ID`
- `VITE_FIREBASE_MESSAGING_SENDER_ID`

### 2. Firebase Setup

1. **Enable Authentication:**
   - Go to Firebase Console → Authentication
   - Enable "Email/Password" sign-in method
   - Create an admin user in the Authentication tab

2. **Create Firestore Database:**
   - Go to Firestore Database
   - Create database in production mode
   - Collections will be created automatically when you add data

3. **Enable Storage:**
   - Go to Storage
   - Get started and set up with default rules
   - Update rules for authenticated uploads:
   ```
   rules_version = '2';
   service firebase.storage {
     match /b/{bucket}/o {
       match /{allPaths=**} {
         allow read: if true;
         allow write: if request.auth != null;
       }
     }
   }
   ```

4. **Set Firestore Rules:**
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /matches/{matchId} {
         allow read: if true;
         allow write: if request.auth != null;
       }
       match /channels/{channelId} {
         allow read: if true;
         allow write: if request.auth != null;
       }
       match /ads/{adId} {
         allow read: if true;
         allow write: if request.auth != null;
       }
     }
   }
   ```

### 3. Running the Application

The application is already configured and running:

```bash
npm run dev
```

This starts both the Express backend and Vite frontend on port 5000.

## Key Features

### 1. Real-Time Data
- Uses Firebase listeners for live updates
- Matches automatically update when marked as "live"
- No page refresh needed for new content

### 2. M3U8 Video Streaming
- HLS.js integration for adaptive streaming
- Full player controls (play, pause, volume, fullscreen)
- Mobile-optimized touch controls
- Auto-hide controls for immersive viewing

### 3. Admin Panel
- Secure Firebase Authentication
- CRUD operations for matches, channels, and ads
- File upload to Firebase Storage for logos and images
- Real-time preview of all content

### 4. Mobile-First Design
- Responsive layout for all screen sizes
- Bottom navigation for easy thumb access
- Touch-optimized controls and buttons
- Dark mode support with theme toggle

## Admin Access

To access the admin panel:

1. Navigate to `/admin` or click "Admin" in the bottom navigation
2. Sign in with your Firebase Authentication credentials
3. Manage matches, channels, and ads from the three tabs

**Default Admin Setup:**
- Create an admin user in Firebase Authentication
- Use those credentials to log in

## Adding Content

### Adding a Match:
1. Go to Admin → Matches tab
2. Click "Add New Match"
3. Fill in team names, date, time, and stream URL
4. Check "Live Now" if the match is currently live
5. Click "Add Match"

### Adding a Channel:
1. Go to Admin → Channels tab
2. Click "Add New Channel"
3. Enter channel name
4. Upload a logo (stored in Firebase Storage)
5. Enter M3U8 stream URL
6. Click "Add Channel"

### Adding an Ad:
1. Go to Admin → Ads tab
2. Click "Add New Ad"
3. Enter ad title
4. Upload ad image
5. Optional: Add target URL for clickable ads
6. Select position (home banner, video player, or channel list)
7. Click "Add Ad"

## Stream URL Format

The app supports M3U8 (HLS) streaming format. Stream URLs should be in the format:
```
https://example.com/path/to/stream.m3u8
```

For testing, you can use this sample stream:
```
https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8
```

## Development Notes

### Dark Mode
- Implemented with Tailwind's dark mode class strategy
- Theme preference saved to localStorage
- Toggle available in header

### State Management
- Firebase listeners for real-time data
- Local state for UI interactions
- No global state management needed (data comes directly from Firebase)

### Performance
- Lazy loading for video player
- Optimized images with proper sizing
- Real-time listeners automatically clean up on unmount

## Deployment

The app is ready for deployment on Replit:

1. Click the "Publish" button in Replit
2. Your app will be available at your Replit domain
3. Firebase will work seamlessly in production

**Important:** Make sure to add your production domain to Firebase:
- Go to Firebase Console → Authentication → Settings
- Add your Replit domain to "Authorized domains"

## Future Enhancements

- Push notifications for match start times
- User favorites and watchlist
- Match highlights and replay functionality
- Multi-language support
- Advanced analytics dashboard
- Comment/chat system for live matches

## Troubleshooting

### Video not playing:
- Check if the M3U8 URL is accessible
- Ensure CORS is enabled on the streaming server
- Try the test stream URL provided above

### Admin login fails:
- Verify Firebase Authentication is enabled
- Check if Email/Password provider is active
- Ensure admin user is created in Firebase Console

### Data not loading:
- Check Firebase connection in browser console
- Verify Firestore rules allow read access
- Check if collections exist in Firestore

## License

This project is built for educational and demonstration purposes.
