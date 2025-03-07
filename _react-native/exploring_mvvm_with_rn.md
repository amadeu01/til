# MVVM with Zustand in React Native

Exploring the integration of MVVM (Model-View-ViewModel) architecture with Zustand state management in a React Native application, specifically handling authentication and session management.

## Project Structure

```
src
├── services
│   └── AuthService.ts
├── stores
│   └── sessionStore.ts
├── viewModels
│   └── useAuthViewModel.ts
├── views
│   ├── Splash.tsx
│   ├── Login.tsx
│   └── Home.tsx
```

## Implementation

### 1. Session Store (Zustand)

Responsible for holding session-related state globally:

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

### 2. Authentication Service

Encapsulates the authentication logic:

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

export class AuthService {
  static async login(credentials: LoginCredentials): Promise<AuthResponse> {
    const response = await axios.post<AuthResponse>('/api/login', credentials);
    return response.data;
  }

  static async validateToken(token: string): Promise<boolean> {
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

### 3. ViewModel (MVVM)

Custom hook encapsulating authentication logic:

**viewModels/useAuthViewModel.ts**
```typescript
import { useState } from 'react';
import { AuthService, LoginCredentials } from '../services/AuthService';
import { useSessionStore } from '../stores/sessionStore';

export const useAuthViewModel = () => {
  const { setUser, clearUser, user } = useSessionStore();
  const [isLoading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const login = async (credentials: LoginCredentials) => {
    setLoading(true);
    setError(null);
    try {
      const authUser = await AuthService.login(credentials);
      setUser(authUser);
    } catch {
      setError('Invalid email or password.');
      clearUser();
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    clearUser();
  };

  const isAuthenticated = () => !!user?.token;

  return { user, login, logout, isLoading, error, isAuthenticated };
};
```

### 4. Views

#### Splash View

Determines navigation based on authentication status:

**views/Splash.tsx**
```tsx
import React, { useEffect } from 'react';
import { View, ActivityIndicator } from 'react-native';
import { useAuthViewModel } from '../viewModels/useAuthViewModel';
import { useNavigation } from '@react-navigation/native';
import { AuthService } from '../services/AuthService';

const Splash: React.FC = () => {
  const { user, logout } = useAuthViewModel();
  const navigation = useNavigation();

  useEffect(() => {
    const checkSession = async () => {
      if (user?.token && await AuthService.validateToken(user.token)) {
        navigation.reset({ index: 0, routes: [{ name: 'Home' }] });
      } else {
        logout();
        navigation.reset({ index: 0, routes: [{ name: 'Login' }] });
      }
    };

    checkSession();
  }, []);

  return (
    <View className="flex-1 justify-center items-center">
      <ActivityIndicator size="large" />
    </View>
  );
};

export default Splash;
```

#### Login View

Handles user login:

**views/Login.tsx**
```tsx
import React, { useState } from 'react';
import { View, TextInput, Button, Text, ActivityIndicator } from 'react-native';
import { useAuthViewModel } from '../viewModels/useAuthViewModel';

const Login: React.FC = () => {
  const { login, isLoading, error } = useAuthViewModel();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
    <View className="p-4">
      <TextInput placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput placeholder="Password" value={password} onChangeText={setPassword} secureTextEntry />
      {error && <Text className="text-red-500">{error}</Text>}
      {isLoading ? <ActivityIndicator /> : <Button title="Login" onPress={() => login({ email, password })} />}
    </View>
  );
};

export default Login;
```

#### Home View

Displays authenticated user info:

**views/Home.tsx**
```tsx
import React from 'react';
import { View, Text, Button } from 'react-native';
import { useAuthViewModel } from '../viewModels/useAuthViewModel';

const Home: React.FC = () => {
  const { user, logout } = useAuthViewModel();

  return (
    <View className="flex-1 p-4">
      <Text className="text-xl font-bold">Welcome, {user?.email}</Text>
      <Button title="Logout" onPress={logout} />
    </View>
  );
};

export default Home;
```

This approach effectively leverages MVVM principles alongside Zustand for scalable, maintainable state management.

