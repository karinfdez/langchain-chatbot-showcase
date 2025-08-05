# LumaTech Knowledge Chatbot

## Note  
This repository is a public showcase of a private project. The actual source code is private, but this outlines the architecture, technologies used, and demo links.

## Live Demo

Try out the live demo: [LumaTech Chatbot](https://langchain-chatbot-pi.vercel.app/)


## Project Description

The application implements a modern split-screen interface with company information displayed on the left and an interactive chatbot on the right. The chatbot answers questions about a fictional company called LumaTech using information stored in a Supabase vector database. It is built using Next.js, LangChain JS, and Supabase to implement a Retrieval-Augmented Generation (RAG) architecture


### How it Works

This chatbot uses OpenAIEmbeddings to convert user questions into vector representations, which are then matched against stored document embeddings in a Supabase table using the SupabaseVectorStore. The most relevant documents are retrieved via vector similarity search using a stored procedure (match_documents). Both the user's original question and the retrieved documents are passed to the ChatOpenAI language model. LangChain's PromptTemplate and RunnableSequence components help structure this process, enabling the LLM to generate natural-language responses grounded in the retrieved data.


## Tech Stack

- **Next.js**: React framework for building the web application
- **LangChain.js**: Framework for building LLM applications
- **OpenAI**: For embeddings and chat completions
- **Supabase**: Vector database for storing and retrieving embeddings
- **Vector Search**: Using similarity search for retrieving relevant documents
- **Upstash Redis**: Serverless Redis database for rate limiting

## Implementation Details

### Components

1. **ChatBox Component**:
   - Custom React chatbot with message history state management
   - Async API calls to the question-answering backend
   - Loading indicators during API requests

2. **TextViewer Component**:
   - Displays the company information text from the source file
   - Custom formatting for headings, lists, and paragraphs
   - Loading state management

### API Routes

1. **/api/load-text**:
   - Processes the company-info.txt file and splits it into chunks
   - Embeds the chunks using OpenAI embeddings
   - Stores them in a Supabase vector store

2. **/api/find-match**:
   - Takes user questions and retrieves relevant documents from Supabase
   - Uses LangChain's RunnableSequence to structure the LLM prompt
   - Returns formatted answers based on the retrieved content

3. **/api/get-text-content**:
   - Retrieves the raw text content from company-info.txt for display

### Security
This application implements secure API endpoints with request signing and timestamp validation to prevent unauthorized access and replay attacks. The client automatically handles authentication, so no additional setup is needed for normal usage.

Note: Environment variables for security settings must be properly configured in both development and production environments.

### Rate Limiting

The application implements rate limiting using Upstash Redis to prevent abuse and control API usage:

- Uses the `@upstash/ratelimit` package with fixed window rate limiting strategy
- Restricts to 7 API requests per IP address per day
- Includes analytics for monitoring rate limit usage
- Configurable prefix for Redis keys (`rate-limit:<identifier>`)

You can view and manage the Redis database through the [Upstash Console](https://console.upstash.com/)


## Getting Started

### Prerequisites

- Node.js 16+ and npm/yarn
- Supabase account with vector extension enabled
- OpenAI API key

### Setup

1. Clone the repository

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables by creating a `.env.local` file:
```
OPENAI_API_KEY=your_openai_api_key
SUPABASE_URL=your_supabase_url
SUPABASE_PRIVATE_KEY=your_supabase_service_role_key
```

4. Create a `company-info.txt` file in the root directory with your company information

5. Load the text data into Supabase by visiting `/api/load-text` in your browser

6. Run the development server:
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.


## Learn More

- [LangChain Documentation](https://js.langchain.com/docs/) - Learn more about LangChain.js
- [Supabase Vector Store](https://supabase.com/docs/guides/ai/vector-columns) - Learn about pgvector extension in Supabase
- [Next.js Documentation](https://nextjs.org/docs) - Learn about Next.js features and API
- [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings) - Learn about text embeddings

