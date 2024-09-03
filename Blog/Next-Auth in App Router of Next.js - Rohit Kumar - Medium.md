

https://medium.com/@rohitkumarkhatri/next-auth-in-app-router-of-next-js-7df037f7a2ad
Next.js has recently released a stable version of App Router enriched with in-built support for layouts, templates, routing, loading, and…


Next.js has recently released a stable version of App Router enriched with in-built support for layouts, templates, routing, loading, and error handling.

The current version of Next-Auth documentation is published with examples of the Page Router of Next.js. This document share approach to setting up Next.js with the Next-Auth App Router.

## **What is covered in this document?**

## Set up the Next.js app with the App router.

Follow this [document](https://nextjs.org/docs/getting-started/installation) to create a basic next.js app with an app router.

![](https://miro.medium.com/v2/resize:fit:700/1*rmTwm4P20YEDLD9mM11Zlw.png)

Next.js app with App router.

## Install Next-Auth

npm install next-auth

## Configuration of Github Provider

[Create a GitHub App first](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app), providing you a client Id and Secret. Add a file auth.js in the lib folder.

Note: Generate a secret to add as NEXTAUTH_SECRET, it must be added in the auth options.

openssl rand -base64 32  
// output  
// mQ46qpFwfE1BHuqMC+qlm19qBAD9fVPgh28werwe3ASFlAfnKjM=

next. config.js

/** @type {import('next').NextConfig} */  
const nextConfig = {  
  images: {  
    remotePatterns: [  
      {  
        protocol: 'https',  
        hostname: 'i.imgur.com',  
      },  
    ],  
  },  
  env: {  
    GITHUB_APP_CLIENT_ID: '919b87qa4sdfs1qdc44f53baf9',  
    GITHUB_APP_CLIENT_SECRET: '2aeq98df3f8cwqerc2d03a8360e993c115ba8d5f71de9',  
    NEXTAUTH_SECRET: 'mQ46qpFwfE1BHuqMC+qlm19qBAD9fVPgh28werwe3ASFlAfnKjM=',  
  },  
};  
module.exports = nextConfig;

  
import { NextAuthOptions } from 'next-auth';  
import GithubProvider from 'next-auth/providers/github';  
export const authOptions: NextAuthOptions = {  
  // Secret for Next-auth, without this JWT encryption/decryption won't work  
  secret: process.env.NEXTAUTH_SECRET,    
  // Configure one or more authentication providers  
  providers: [  
    GithubProvider({  
      clientId: process.env.GITHUB_APP_CLIENT_ID as string,  
      clientSecret: process.env.GITHUB_APP_CLIENT_SECRET as string,  
    }),  
  ],  
};

## Add Handler for Next-Auth

Create a file `route.js` in the folder `/app/auth/[…nextauth]`
![[Pasted image 20240821014834.png]]

import NextAuth from 'next-auth';  
import { authOptions } from '@/app/lib/auth';  
const handler = NextAuth(authOptions);  
export { handler as GET, handler as POST };

## Sign in and Sign out using next-auth

All requests to any of these paths`/api/auth/*` (`signIn`, `callback`, `signOut`, etc.) will be handled by NextAuth.js.

Once you click the button, log in, and authorize the app, it will get a cookie in the session.

![](https://miro.medium.com/v2/resize:fit:700/1*hL--1CcNAdaRAo1mh_rv7g.png)

Next-Auth default Sign in page

After successful sign-in.

![](https://miro.medium.com/v2/resize:fit:700/1*wAZUrApa9RpDMur5PLpReA.png)

Next-Auth token

## Sign Out

use this link ` /api/auth/signout ` to sign out.

## Session in API routes.

API route handlers are considered server components. It can be a static or dynamic handler but these are always on the server side.

import { authOptions } from '@/app/lib/auth';  
import { getServerSession } from 'next-auth';  
import { NextResponse } from 'next/server';  
export async function GET(request: Request) {  
  const session = await getServerSession(authOptions);  
  console.log(session);  
  return NextResponse.json({  
    id: 1,  
  });  
}

Output

{  
  user: {  
    name: 'Rohit Kumar Khatri',  
    email: 'er.rohitkumar@outlook.com',  
    image: 'https://avatars.githubusercontent.com/u/34018015?v=4'  
  }  
}

## Session in Server Components

It is also similar in all server components. Use the same function as mentioned above.

await getServerSession(authOptions);

## Session in Client Components

First, add Session Provider in the Root Layout

'use client';  
import { SessionProvider } from 'next-auth/react';  
import { ReactNode } from 'react';  
export default function NextAuthProvider({  
  children,  
}: {  
  children: ReactNode;  
}) {  
  return <SessionProvider>{children}</SessionProvider>;  
}

import { useSelectedLayoutSegments } from 'next/navigation';  
import './globals.css';  
import { Inter } from 'next/font/google';  
import Footer from '@/app/components/Footer';  
import Navbar from '@/app/components/Navbar';  
import NextAuthProvider from '@/app/context/NextAuthProvider';  
import { ReactNode } from 'react';  
const inter = Inter({ subsets: ['latin'] });  
export default function RootLayout({ children }: { children: ReactNode }) {  
  return (  
    <html lang="en">  
      <body className={`inter.className`}>  
        <NextAuthProvider>  
          <div className="w-10/12 m-auto text-center bg-white flex flex-col min-h-screen">  
            <div>  
              <Navbar />  
            </div>  
            <div className="grow">{children}</div>  
            <Footer />  
          </div>  
        </NextAuthProvider>  
      </body>  
    </html>  
  );  
}

Now use session in any client component.

'use client';  
import { useSession } from 'next-auth/react';  
export default function ClientComponent() {  
  const { data: session, status } = useSession();  
  return (  
    <div>  
      ClientComponent {status}{' '}  
      {status === 'authenticated' && session.user?.name}  
    </div>  
  );  
}