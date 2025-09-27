This is a full-stack e-commerce web application built with the **Next.js App Router**. It showcases a modern, high-performance online store with product browsing, a fully functional shopping cart, and a complete checkout experience powered by **Stripe**.

The project uses **Zustand** for efficient global state management and **Tailwind CSS** for a sleek, responsive user interface.

-----

Key Features

  * **Modern Tech Stack:** Built with Next.js 13+ (App Router) for server-side rendering, static site generation, and optimized performance.
  * **Product Catalog:** Displays products fetched from a data source, with dedicated pages for individual product details.
  * **Dynamic Routing:** Uses Next.js dynamic routes to generate unique pages for each product (e.g., `/product/[id]`).
  * **Shopping Cart:** A fully persistent shopping cart that allows users to:
      * Add and remove items.
      * Increase or decrease item quantity.
      * View a summary of all items and the total price.
  * **State Management:** Utilizes **Zustand**, a lightweight and powerful state management library, to handle the cart's state across the entire application.
  * **Secure Payments:** Integrates **Stripe Checkout** for a secure, seamless, and production-ready payment process.
  * **Responsive Design:** Styled with **Tailwind CSS**, ensuring the application looks great and functions perfectly on all devices, from desktops to mobile phones.
  * **Notifications:** Uses `react-hot-toast` to provide users with non-intrusive feedback for actions like adding an item to the cart.

-----

Tech Stack

  * **Framework:** [Next.js](https://nextjs.org/)
  * **Language:** [TypeScript](https://www.typescriptlang.org/)
  * **Styling:** [Tailwind CSS](https://tailwindcss.com/)
  * **Payment Processing:** [Stripe](https://stripe.com/)
  * **UI Notifications:** [React Hot Toast](https://react-hot-toast.com/)

-----

Getting Started

Follow these instructions to get a copy of the project up and running on your local machine for development and testing purposes.

Prerequisites

Make sure you have the following installed on your system:

  * [Node.js](https://nodejs.org/en/) (v18.x or later)
  * [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)
  * A [Stripe](https://dashboard.stripe.com/register) account to get API keys.

Installation & Setup

1.  **Clone the repository:**

    ```sh
    git clone https://github.com/your-username/your-repo-name.git
    cd your-repo-name
    ```

2.  **Install dependencies:**

    ```sh
    npm install
    ```

    or

    ```sh
    yarn install
    ```

3.  **Set up environment variables:**
    Create a file named `.env.local` in the root of your project and add your Stripe API keys. You can find these in your [Stripe Developer Dashboard](https://dashboard.stripe.com/test/apikeys).

    ```.env.local
    NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_YOUR_PUBLISHABLE_KEY
    STRIPE_SECRET_KEY=sk_test_YOUR_SECRET_KEY
    ```

      * `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`: This is your client-side key.
      * `STRIPE_SECRET_KEY`: This is your server-side key. **Never expose this key publicly.**

4.  **Run the development server:**

    ```sh
    npm run dev
    ```

    or

    ```sh
    yarn dev
    ```

The application should now be running on [http://localhost:3000](https://www.google.com/search?q=http://localhost:3000).

-----

## \#\# Project Structure Explained

The project uses the Next.js App Router, which organizes the application's structure around folders within the `/app` directory.

```
/
├── app/
│   ├── api/stripe/         # API route for handling Stripe Checkout session creation
│   ├── cart/               # Route for the shopping cart page
│   ├── product/[id]/       # Dynamic route for individual product pages
│   ├── layout.tsx          # Root layout for the application
│   └── page.tsx            # The homepage component
├── components/
│   ├── Cart.tsx            # The main cart component (drawer/modal)
│   ├── CheckoutButton.tsx  # Component for initiating the Stripe checkout
│   ├── Navbar.tsx          # The website's navigation bar
│   └── Product.tsx         # The card component for displaying a single product
├── lib/
│   └── stripe.ts           # Stripe server-side instance initialization
├── public/                 # Static assets like images
└── store/
    └── useCartStore.ts     # Zustand store for managing cart state
```

  * **`/app`**: Contains all the routes, pages, and layouts. Each folder represents a URL segment.
  * **`/app/api`**: This is where server-side API logic lives. The `/api/stripe` route is responsible for securely communicating with the Stripe API to create a payment session.
  * **`/components`**: Holds all the reusable React components used throughout the application.
  * **`/lib`**: Contains helper functions and library initializations, such as the server-side Stripe instance.
  * **`/store`**: This directory contains the Zustand state management logic. `useCartStore.ts` defines the state and actions for the shopping cart (e.g., `addProduct`, `removeProduct`).
