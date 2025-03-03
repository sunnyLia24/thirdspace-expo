# Third Spaces - Location-Based Dating App

## Overview
Third Spaces is a location-based dating application that connects users in real-time through an interactive 3D map. The app focuses on facilitating authentic in-person connections at public "third spaces" such as cafes, parks, and co-working locations.

## Features

### 1. Welcome Experience
- **Clean & Minimalistic Design**: A simple, elegant interface greets new users
- **Authentication Options**: Sign up or log in using email

### 2. Personalized Onboarding
Upon signing up, users create their profile by answering:

1. **Personal Details**:
   - Full name (First & Last)
   - Gender identity (Female, Male, Other)
   - Date of birth (Month, Day, Year)
   - Height (in ft.)

2. **Preferences & Lifestyle**:
   - Interested in (Women, Men, Anyone)
   - Religious beliefs (Select from options or custom entry)
   - Political stance (Select from options or custom entry)
   - Lifestyle habits (Drinking/Smoking preferences)

### 3. Interactive Map Interface
The core experience centers around a 3D map powered by Mapbox displaying:

- **User Representation**: User appears as a floating ball with ripple effect (similar to Pokémon GO)
- **Real-Time Discovery**: Other active users appear on the map in their actual locations
- **Interactive Profiles**: Tap on other users to view profiles and initiate interaction
- **Partner Venues**: Featured locations offering exclusive perks for app users

#### Interface Elements

**Top Navigation Bar**:
- **Top Right**: "Do Not Disturb" toggle to pause new interactions
- **Top Left**: Settings access for account management and preferences

**Bottom Navigation Bar**:
1. **Rewind Icon**: Undo last interaction
2. **Messages Icon**: Access conversation inbox
3. **Sparkles Icon**: View potential matches and nearby interested users
4. **Profile Icon**: View and edit personal profile

> Each navigation page includes an X button (top right) to return to the map view

### 4. Additional Capabilities

- **Dynamic Location Updates**: See users in your vicinity as they move in real-time
- **Comprehensive Privacy Controls**: 
  - Toggle visibility status
  - Limit interaction types
  - Enable "Do Not Disturb" mode
- **Secure Messaging**: Real-time chat between matched users
- **Preference Filtering**: Filter visible users based on onboarding preferences

### 5. Planned Enhancements

- **Event Integration**: Discover and RSVP to social events at nearby venues
- **Exclusive Benefits**: Access to special offers and dedicated areas at partner locations
- **Gamified Social Experience**: Earn badges and rewards for meeting new people

## Technology Stack
- Frontend: React Native with TypeScript, Expo, and Expo Router
- Maps: Mapbox 3D
- Location Services: Real-time geolocation tracking
- Backend/Database: Supabase 
- UI Framework: React Native Paper
- AI Processing: DeepSeek

## Database Schema

### Tables

#### 1. users
- `id` (UUID, PK) - Primary identifier
- `email` (string, unique) - User's email for authentication
- `created_at` (timestamp) - Account creation date
- `last_active` (timestamp) - Last login/activity
- `is_visible` (boolean) - If user is visible on the map
- `do_not_disturb` (boolean) - DND status
- `fcm_token` (string, nullable) - Firebase Cloud Messaging token for notifications
- `onboarding_completed` (boolean) - Whether user completed onboarding

#### 2. profiles
- `id` (UUID, PK, FK to users.id) - References users table
- `first_name` (string) - User's first name
- `last_name` (string) - User's last name
- `gender` (enum: 'female', 'male', 'other') - Gender identity
- `birth_date` (date) - Date of birth
- `height_ft` (decimal) - Height in feet
- `interested_in` (enum: 'women', 'men', 'anyone') - Dating preference
- `religious_belief` (string, nullable) - Religious belief
- `political_belief` (string, nullable) - Political stance
- `drinks` (boolean) - Whether user drinks alcohol
- `smokes` (boolean) - Whether user smokes
- `bio` (text, nullable) - User's biography
- `avatar_url` (string, nullable) - Profile picture URL

#### 3. user_locations
- `id` (UUID, PK) - Unique identifier
- `user_id` (UUID, FK to users.id) - References users table
- `longitude` (decimal) - Current longitude
- `latitude` (decimal) - Current latitude
- `last_updated` (timestamp) - When location was last updated
- `accuracy` (decimal) - Location accuracy in meters

#### 4. user_photos
- `id` (UUID, PK) - Unique identifier
- `user_id` (UUID, FK to profiles.id) - References profiles table
- `photo_url` (string) - URL to photo
- `created_at` (timestamp) - When photo was uploaded
- `order` (integer) - Display order of photos

#### 5. matches
- `id` (UUID, PK) - Unique identifier
- `user1_id` (UUID, FK to users.id) - First user in match
- `user2_id` (UUID, FK to users.id) - Second user in match
- `created_at` (timestamp) - When match occurred
- `status` (enum: 'pending', 'accepted', 'rejected') - Match status

#### 6. messages
- `id` (UUID, PK) - Unique identifier
- `match_id` (UUID, FK to matches.id) - References matches table
- `sender_id` (UUID, FK to users.id) - User who sent the message
- `content` (text) - Message content
- `created_at` (timestamp) - When message was sent
- `read_at` (timestamp, nullable) - When message was read

#### 7. venues
- `id` (UUID, PK) - Unique identifier
- `name` (string) - Venue name
- `type` (enum: 'cafe', 'park', 'coworking', 'restaurant', 'bar', 'other') - Venue type
- `longitude` (decimal) - Venue longitude
- `latitude` (decimal) - Venue latitude
- `description` (text) - Venue description
- `logo_url` (string, nullable) - Venue logo URL
- `is_partner` (boolean) - Whether venue is a partner location

#### 8. venue_perks
- `id` (UUID, PK) - Unique identifier
- `venue_id` (UUID, FK to venues.id) - References venues table
- `title` (string) - Perk title
- `description` (text) - Perk description
- `valid_from` (timestamp) - When perk becomes valid
- `valid_until` (timestamp, nullable) - When perk expires

#### 9. events
- `id` (UUID, PK) - Unique identifier
- `venue_id` (UUID, FK to venues.id) - References venues table
- `title` (string) - Event title
- `description` (text) - Event description
- `start_time` (timestamp) - Event start time
- `end_time` (timestamp) - Event end time
- `image_url` (string, nullable) - Event image

#### 10. event_attendees
- `id` (UUID, PK) - Unique identifier
- `event_id` (UUID, FK to events.id) - References events table
- `user_id` (UUID, FK to users.id) - References users table
- `rsvp_status` (enum: 'going', 'interested', 'not_going') - RSVP status

### Indexes
- `users`: email (unique)
- `user_locations`: user_id, (latitude, longitude) for geospatial queries
- `matches`: (user1_id, user2_id) composite unique
- `messages`: match_id, created_at
- `venues`: (latitude, longitude) for geospatial queries

### Relationships
- One-to-one between `users` and `profiles`
- One-to-many between `users` and `user_locations`
- One-to-many between `profiles` and `user_photos`
- Many-to-many between `users` through `matches`
- One-to-many between `matches` and `messages`
- One-to-many between `venues` and `venue_perks`
- One-to-many between `venues` and `events`
- Many-to-many between `users` and `events` through `event_attendees`

## Application Structure

```
third-spaces/
├── app/                      # Expo Router app directory
│   ├── (auth)/               # Authentication routes
│   │   ├── login.tsx         # Login screen
│   │   ├── signup.tsx        # Signup screen
│   │   └── _layout.tsx       # Auth layout
│   ├── (onboarding)/         # Onboarding routes
│   │   ├── personal-info.tsx # Personal details screen
│   │   ├── preferences.tsx   # User preferences screen
│   │   └── _layout.tsx       # Onboarding layout
│   ├── (main)/               # Main app routes
│   │   ├── index.tsx         # Map/Home screen
│   │   ├── messages/         # Messages screens
│   │   │   ├── index.tsx     # Messages list
│   │   │   └── [id].tsx      # Individual conversation
│   │   ├── sparkles.tsx      # Matches & nearby users
│   │   ├── profile.tsx       # User profile
│   │   └── _layout.tsx       # Main app layout with tab bar
│   ├── modals/               # Modal screens
│   │   ├── settings.tsx      # Settings modal
│   │   └── user-profile.tsx  # Other user profile modal
│   └── _layout.tsx           # Root layout
├── assets/                   # Static assets
│   ├── fonts/                # Custom fonts
│   ├── images/               # Image assets
│   └── animations/           # Lottie animations
├── components/               # Reusable components
│   ├── common/               # Generic UI components
│   │   ├── Button.tsx        # Custom button
│   │   ├── Input.tsx         # Form input
│   │   └── ...
│   ├── map/                  # Map-related components
│   │   ├── MapView.tsx       # Mapbox component
│   │   ├── UserMarker.tsx    # User representation on map
│   │   └── ...
│   ├── profile/              # Profile components
│   ├── messages/             # Messaging components
│   └── onboarding/           # Onboarding components
├── constants/                # App constants
│   ├── Colors.ts             # Color definitions
│   ├── Layout.ts             # Layout constants
│   └── ...
├── context/                  # React Context providers
│   ├── AuthContext.tsx       # Authentication context
│   ├── LocationContext.tsx   # User location context
│   └── ...
├── hooks/                    # Custom React hooks
│   ├── useAuth.ts            # Authentication hooks
│   ├── useLocation.ts        # Location hooks
│   └── ...
├── lib/                      # Shared libraries
│   ├── supabase.ts           # Supabase client
│   ├── mapbox.ts             # Mapbox utilities
│   └── ...
├── services/                 # API and external services
│   ├── api.ts                # API client
│   ├── location.ts           # Location services
│   └── ...
├── types/                    # TypeScript type definitions
│   ├── schema.ts             # Database schema types
│   ├── navigation.ts         # Navigation types
│   └── ...
├── utils/                    # Utility functions
│   ├── formatters.ts         # Data formatting utilities
│   ├── validators.ts         # Input validation
│   └── ...
├── .gitignore                # Git ignore file
├── app.json                  # Expo app config
├── babel.config.js           # Babel configuration
├── eas.json                  # EAS Build configuration
├── package.json              # Node dependencies
├── tsconfig.json             # TypeScript configuration
└── README.md                 # Project documentation
```