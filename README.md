# Frontend System Design Fundamentals: A Guide with TikTok's Web Application Example

This guide outlines the essential concepts of frontend system design with a practical example using TikTok’s web application. Each section includes a brief explanation of the concept, how it relates to TikTok's frontend architecture, and specific examples from TikTok’s features to provide deeper insights.

## Table of Contents
1. [Requirements](#requirements)
   - Functional
   - Non-functional
2. [High-Level Architecture](#high-level-architecture)
3. [Data Model](#data-model)
4. [API Model](#api-model)
5. [Optimization & Performance](#optimization-performance)

---

## Requirements

### Functional Requirements
- **Definition**: Functional requirements specify what the system should do, such as key features and behaviors.
- **TikTok Example**: TikTok’s main functional requirements include displaying a dynamic feed with infinite scrolling, where each item represents a user post that can include comments.
- **Concrete Example**: The For You Page (FYP) displays a continuous list of user-generated videos with likes, comments, and share options.
- **Challenges**:
  - Efficiently loading new videos as the user scrolls.
  - Minimizing response times for comments and reactions.

### Non-functional Requirements
- **Definition**: Non-functional requirements specify the quality attributes of the system, such as usability, performance, and scalability.
- **TikTok Example**:
  - **Mobile Friendly**: TikTok’s interface adapts seamlessly to mobile screens.
  - **Accessibility**: Support for screen readers, alternative text for videos, and clear navigation.
  - **Performance**: Optimized for fast load times and smooth video playback.
  - **Expected Volume**: High user traffic with millions of concurrent users.
- **Challenges**:
  - Balancing high performance with large-scale user interaction.
  - Ensuring consistent experience across devices.

---

## High-Level Architecture

- **Definition**: The high-level architecture is an overview of the system's main components and their interactions.
- **TikTok Example**:
  - **Frontend Components**: Header, Sidebar, Feed, Video Player, Comment Section, and User Profile.
  - **Backend Components**: Content Delivery Network (CDN) for fast video serving, API Gateway for client-server communication, Database for storing user data, and Video Processing Service.
- **Concrete Example**: When a user loads the FYP, the client fetches data for the next set of videos, which the CDN caches for fast loading.
- **Challenges**:
  - Maintaining smooth performance while fetching large video files.
  - Managing the real-time display of likes and comments without overloading the client.

```mermaid
graph LR
    A[Client (Browser)] -- Requests --> B[API Gateway]
    B -- Requests --> C[Backend Services]
    C -- Fetches --> D[Database]
    D -- Data --> B
    B -- Response --> A
```

---

## Data Model

- **Definition**: The data model defines the structure of data required for the application.
- **TikTok Example**:
  - **Post Model**: Includes fields like `id`, `user_id`, `video_url`, `caption`, `like_count`, `comment_count`.
  - **Comment Model**: Includes `comment_id`, `post_id`, `user_id`, `content`, `timestamp`.
- **Concrete Example**: The Post model supports infinite scroll by limiting the number of posts fetched per API call, reducing data payloads.
- **Challenges**:
  - Efficiently structuring data to minimize loading times.
  - Handling high volumes of likes and comments on popular posts without excessive database querying.

---

## API Model

- **Definition**: The API model describes the method of communication between frontend and backend, defining how data is fetched and updated.
- **TikTok Example**:
  - **Polling**: Periodic checks for updates on views and likes.
  - **GraphQL**: Used to fetch only required fields like video URLs, captions, and comments.
  - **WebSocket**: For real-time updates on likes and comments without constant polling.
- **Concrete Example**: Using WebSocket for real-time updates ensures users see live counts on likes and comments as they interact.
- **Challenges**:
  - Balancing frequent updates with performance.
  - Ensuring stable connections for WebSocket under heavy traffic.

---

## Optimization & Performance

### Multiplexing

- **Definition**: Multiplexing refers to sending multiple streams of data over a single connection, reducing overhead.
- **TikTok Example**: Multiplexing can allow multiple videos to load concurrently, making feed scrolling smoother.
- **Concrete Example**: Multiplexing can prefetch videos based on the user’s scrolling speed, reducing buffer times.
- **Challenges**:
  - Managing connection limits in a browser.
  - Ensuring smooth preloading without sacrificing page speed.

### Network Optimization
1. **Compression Headers**: Use Gzip to reduce API response size.
2. **Caching**: Cache static assets like profile pictures.
3. **Batch Requests**: Combine multiple API calls to reduce round trips.
4. **Image Optimization**: Serve images at appropriate resolutions.
5. **Bundle Splitting**: Load essential scripts first and defer others.
- **TikTok Example**: TikTok uses caching and bundle splitting to ensure fast video and profile picture loading.

### Rendering Optimization
1. **Mobile Friendly**: Adaptive layouts for various screen sizes.
2. **Concurrent Requests**: Fetch comments and likes simultaneously.
3. **Application Cache**: Store user data locally for offline usage.
4. **Error States**: Indicate when videos or comments fail to load.
5. **Server-Side Rendering (SSR)**: Improves SEO and initial load time.
6. **Above-the-Fold Content**: Prioritize loading visible content first.
7. **Time to Interactive (TTI)**:
   - Measure metrics like **First Contentful Paint** to track loading progress.
   - Log request and component render times.
8. **Preloading**: Load JavaScript only when necessary.
9. **Tree Shaking**: Remove unused code.
10. **Virtualization**: Use virtual lists to improve scroll performance.
- **TikTok Example**: Uses virtualization to load only the visible videos and defer others, optimizing feed scroll performance.

### CSS Optimization
1. **Avoid Reflows**: Minimize layout changes with efficient styling.
2. **CSS Animations**: Use for smoother transitions.
3. **Inline CSS**: Reduce load time by embedding critical styles directly.
- **TikTok Example**: CSS animations provide smooth transitions between videos without reflows that would disrupt scrolling.

### Rate Limiting
1. **Debounce/Throttle**: Limit the frequency of specific operations.
   - **TikTok Example**: Limits comment fetch requests to prevent rapid, unnecessary calls as users type.
  
### Accessibility
1. **REM Font Size**: Ensures scalable, responsive text sizes.
2. **Cross-Device Compatibility**: Works across all major browsers and devices.
3. **HTML5 Semantics**: Improved navigation for screen readers.
4. **ARIA Roles**: Enhance accessibility for screen readers.
- **TikTok Example**: Uses ARIA roles for navigation elements, making the app usable for visually impaired users.

### Security
1. **Cross-Site Scripting (XSS)**: Sanitize inputs to prevent attacks.
2. **Rate Limiting**: Limit repeated actions to prevent abuse.
3. **CORS**: Control cross-origin requests to protect user data.
- **TikTok Example**: Applies CORS policies to secure API endpoints, ensuring only authorized apps and users can access data.
