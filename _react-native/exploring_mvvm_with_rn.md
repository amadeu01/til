# 🚀 MVVM with Zustand in React Native

Exploring the integration of the MVVM (Model-View-ViewModel) architecture with Zustand state management in a React Native application, specifically for handling authentication and session management.

## 📁 Project Structure

```
src
├── 📄 App.tsx
├── 📂 services
│   ├── 🔑 AuthService.ts
│   ├── 🧪 MockAuthService.ts
│   └── 📄 index.ts
├── 📂 stores
│   └── 🌐 sessionStore.ts
├── 📂 viewModels
│   └── 📌 useAuthViewModel.ts
├── 📂 views
│   ├── 🚦 Splash.tsx
│   ├── 🔐 Login.tsx
│   └── 🏠 Home.tsx
```

## 🛠️ Implementation

### 🌐 Session Store (Zustand)

Responsible for global session state management:

**stores/sessionStore.ts**
```typescript
import { create } from 'zustand';

export interface User {
  id: string;
  email: string;
  token: string;
}

interface SessionState {
  user: User | null;
  setUser: (user: User) => void;
  clearUser: () => void;
}

export const useSessionStore = create<SessionState>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  clearUser: () => set({ user: null }),
}));
```

### 🔑 Authentication Service

Encapsulates authentication logic:

**services/AuthService.ts**
```typescript
import axios from 'axios';

export interface LoginCredentials {
  email: string;
  password: string;
}

export interface AuthResponse {
  id: string;
  email: string;
  token: string;
}

export interface IAuthService {
  login(credentials: LoginCredentials): Promise<AuthResponse>;
  validateToken(token: string): Promise<boolean>;
}

export class AuthService implements IAuthService {
  async login(credentials: LoginCredentials): Promise<AuthResponse> {
    const response = await axios.post<AuthResponse>('/api/login', credentials);
    return response.data;
  }

  async validateToken(token: string): Promise<boolean> {
    try {
      await axios.get('/api/validate-token', {
        headers: { Authorization: `Bearer ${token}` },
      });
      return true;
    } catch {
      return false;
    }
  }
}
```

### 🧪 Dependency Injection & Mocking

Define interfaces for flexible service implementations:

**services/MockAuthService.ts**
```typescript
import { IAuthService, AuthResponse, LoginCredentials } from './AuthService';

export class MockAuthService implements IAuthService {
  async login(credentials: LoginCredentials): Promise<AuthResponse> {
    return {
      id: '123',
      email: credentials.email,
      token: 'mock-token',
    };
  }

  async validateToken(token: string): Promise<boolean> {
    return token === 'mock-token';
  }
}
```

**services/index.ts**
```typescript
import { AuthService } from './AuthService';
import { MockAuthService } from './MockAuthService';

const authService = __DEV__ ? new MockAuthService() : new AuthService();

export { authService };
```

### 📌 ViewModel (MVVM)

Custom hook leveraging injected services:

**viewModels/useAuthViewModel.ts**
```typescript
import { useState } from 'react';
import { LoginCredentials } from '../services/AuthService';
import { authService } from '../services';
import { useSessionStore } from '../stores/sessionStore';

export const useAuthViewModel = () => {
  const { setUser, clearUser, user } = useSessionStore();
  const [isLoading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const login = async (credentials: LoginCredentials) => {
    setLoading(true);
    setError(null);
    try {
      const authUser = await authService.login(credentials);
      setUser(authUser);
    } catch {
      setError('Invalid email or password.');
      clearUser();
    } finally {
      setLoading(false);
    }
  };

  const logout = () => clearUser();

  const isAuthenticated = () => !!user?.token;

  return { user, login, logout, isLoading, error, isAuthenticated };
};
```

### 🎨 Creating & Running the App

#### 📄 App Entry (`App.tsx`)

```tsx
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import Splash from './views/Splash';
import Login from './views/Login';
import Home from './views/Home';

const Stack = createStackNavigator();

const App: React.FC = () => (
  <NavigationContainer>
    <Stack.Navigator initialRouteName="Splash">
      <Stack.Screen name="Splash" component={Splash} options={{ headerShown: false }} />
      <Stack.Screen name="Login" component={Login} options={{ headerShown: false }} />
      <Stack.Screen name="Home" component={Home} options={{ headerShown: false }} />
    </Stack.Navigator>
  </NavigationContainer>
);

export default App;
```

### 🌟 Store Initialization & Injection

- Zustand stores are automatically global, requiring no explicit initialization.
- ViewModels directly import and utilize stores through Zustand hooks, simplifying dependency management.

## 🔧 Dependency Injection & Build Flavors

- Easily swap implementations for different environments (development, testing, production).
- Enhances modularity and simplifies testing.

## 🎯 Summary

Leveraging MVVM principles, Zustand state management, and dependency injection creates a scalable, maintainable, and easily testable React Native application. This setup supports rapid development with minimal boilerplate. 🚀📱✨

