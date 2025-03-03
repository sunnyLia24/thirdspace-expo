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
- Frontend: React Native
- Maps: Mapbox 3D
- Location Services: Real-time geolocation tracking
- Backend: [To be determined] 