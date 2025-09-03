# 🤖 n8n AI Agent Mastery Course 2025

Welcome to the most comprehensive n8n AI Agent course! Build powerful automation workflows and intelligent AI agents using n8n's visual workflow builder.

## 🎯 Course Overview

This hands-on course teaches you to create sophisticated AI agents and automation workflows using n8n. From basic chatbots to advanced RAG (Retrieval-Augmented Generation) systems, you'll master the art of building intelligent automation solutions.

### 🎓 **What You'll Learn**

**🧠 AI Theory & Fundamentals**
- Master the theory of LLMs and AI agents through clear visual examples
- Summarize key AI‑agent research and distinguish between AI agents, workflows, automation, and AI automation
- Explain detailed definitions of APIs, servers, hosting, function calling, and test‑time compute
- Differentiate between self‑hosting, cloud hosting, and various other hosting types

**🚀 n8n Deployment & Management**
- Deploy and manage n8n via self‑hosting (Node.js, Docker), cloud platforms, and hosting providers like Hostinger
- Expose webhook endpoints by tunneling a self‑hosted n8n Docker instance
- Configure credentials in self‑hosted n8n instances
- Debug n8n workflows and integrate with external applications

**🤖 AI Agent Development**
- Build robust n8n AI agents locally at no cost
- Differentiate among user, system, and assistant message roles in LLM interactions
- Master prompt engineering and context engineering techniques
- Develop a tool to automatically generate system messages for AI agents
- Develop voice‑enabled AI agents using n8n
- Master the design and implementation of over 100 AI agents

**📊 Advanced AI Systems**
- Understand Retrieval‑Augmented Generation (RAG) in depth and develop powerful RAG‑driven AI agents
- Optimize RAG chatbots using embeddings, chunking, and related techniques
- Develop advanced AI agent solutions, including collaborative agent teams and MCP implementations
- Build multiple sentiment analysis projects

**🔗 Integration & Automation**
- Build practical automation projects using Gmail and Google Workspace apps (e.g., Sheets)
- Integrate WhatsApp, Telegram, calendar, web scraping, and other services with n8n
- Integrate apps with websites and build an AI‑automation-driven business
- Compare pricing plans of major AI platforms (OpenAI, Claude, Gemini, DeepSeek, xAI, etc.)

**🛡️ Security & Optimization**
- Implement strategies to address challenges, ensure security compliance, and protect data
- Identify all memory types in n8n
- Develop premium AI agents for business applications

### 📊 **Course Statistics**
- **🎬 40+ Video Tutorials**: Step-by-step implementation guides
- **🤖 10 Complete Projects**: From beginner to advanced AI agents
- **⏱️ 20+ Hours**: Comprehensive hands-on learning
- **🏆 Production-Ready**: Deploy real business solutions

## 🎥 Complete Course Playlist

**[📺 Watch Full Course on YouTube](https://www.youtube.com/playlist?list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v)**

**🎓 If you want to access the course on Udemy for FREE** - [🆓 Get Free Access](https://www.udemy.com/course/mastering-ai-agentic-engineering-with-n8n/?couponCode=CD911814FEB35662C2D9)  
*(Valid for only 1000 people until 09/06/2025)*

## 📋 Prerequisites

- **n8n installation** (we'll cover this in setup guide)
- **Enthusiasm to learn** AI automation!
- **No coding experience required** - n8n is visual!

## 🚀 Quick Start Guide

### 1. **Clone This Repository**
```bash
git clone https://github.com/MohElshamy1994/n8n-course-Projetcs.git
cd n8n-course-Projetcs
```

### 2. **Import Workflows**
1. Open n8n at `http://localhost:5678`
2. Go to **Workflows** → **Import** → **From File**
3. Select any JSON file from the `workflows/` folder
4. Follow along with the corresponding YouTube video

## 🔄 Docker Management

### **Initial Docker Setup**

**🚀 New to n8n Docker Setup?** If you haven't set up n8n in a Docker container yet, **watch this video first** and follow the steps one by one:

> **📺 Complete Setup Guide**: [🎥 n8n Docker Installation Tutorial](https://www.youtube.com/watch?v=VXCC8jHN7zA&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=50)
> 
> **📄 Reference Material**: [📁 Lecture 37 PDF Guide](workflows/37-n8n_Docker_Installation/Lec37.pdf)

This comprehensive tutorial will guide you through the complete Docker installation process before you proceed with updates.

---

### **Updating n8n Docker Container**

To keep your n8n instance up-to-date with the latest features and security patches, follow this step-by-step process:

| Step | Command | Description |
|------|---------|-------------|
| **1. Stop Container** | `docker stop n8n` | Safely stop the running n8n container |
| **2. Remove Container** | `docker rm n8n` | Remove the old container (data persists in volume) |
| **3. Pull Latest Image** | `docker pull docker.n8n.io/n8nio/n8n` | Download the latest n8n Docker image |
| **4. Start New Container** | `docker run -d --name n8n -p 5677:5678 ^`<br/>`  -e WEBHOOK_URL=https://n8ncourse.n8ncourse.cfd ^`<br/>`  -e N8N_DEFAULT_BINARY_DATA_MODE=filesystem ^`<br/>`  -v C:\Docker\n8n-data:/home/node/.n8n ^`<br/>`  docker.n8n.io/n8nio/n8n` | Launch updated n8n with preserved data and settings |

> **📺 Video Tutorial**: [🎥 How to Update n8n Docker Container](https://www.youtube.com/watch?v=VIDEO_ID_PLACEHOLDER) *(Coming Soon)*

> **⚠️ Important Notes:**
> - Your workflows and data are preserved thanks to the volume mount (`-v C:\Docker\n8n-data:/home/node/.n8n`)
> - Always backup your data before updating: `docker cp n8n:/home/node/.n8n C:\Backup\n8n-backup-$(date +%Y%m%d)`
> - The container will be accessible at `http://localhost:5677` after the update

## 🤖 AI Agent Projects

> **📝 Note**: This is not the whole course - projects are updated with time as new content is added!

### 🟢 **Beginner Level Projects**

| Project | Description | Source File | YouTube Video |
|---------|-------------|-------------|---------------|
| **Smart Company Chatbot** | Build an intelligent customer service chatbot for businesses with FAQ handling and lead generation | [📁 Download](workflows/25_Smart_Chatbot_For_Companies.json) | [🎥 Watch](https://www.youtube.com/watch?v=BFi05jLLjMU&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=31) |
| **Company Chatbot with Grok** | Enhanced chatbot using Grok AI for more natural conversations and better context understanding | [📁 Download](workflows/26_Comp_Company_Chatbot_Grok4.json) | [🎥 Watch](https://www.youtube.com/watch?v=exsnquJcSmE&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=32) |

### 🟡 **Intermediate Level Projects**

| Project | Description | Source File | YouTube Video |
|---------|-------------|-------------|---------------|
| **RAG Chatbot System** | Build a Retrieval-Augmented Generation chatbot that can answer questions based on your documents | [📁 Download](workflows/29-RAG_Chatbot.json) | [🎥 Part 1](https://www.youtube.com/watch?v=ntGE0fN8WWM&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=39) • [🎥 Part 2](https://www.youtube.com/watch?v=axOTx3e8660&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=40) |
| **RAG AI Agent** | Advanced AI agent with document processing, vector storage, and intelligent query handling | [📁 Download](workflows/30-RAG_AI_Agent.zip) | [🎥 Watch](https://www.youtube.com/watch?v=K1G86IlyWyE&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=41) |
| **GPT Open Source Agent** | Create your own GPT-like agent using open-source models and custom training data | [📁 Download](workflows/39-GPT-Oss.json) | [🎥 Watch](https://www.youtube.com/watch?v=evFucdQK7kQ&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=52) |

### 🔴 **Advanced Level Projects**

| Project | Description | Source File | YouTube Video |
|---------|-------------|-------------|---------------|
| **High-Accuracy RAG Agent** | Ultra-precise RAG system with advanced chunking, multiple vector stores, and context optimization | [📁 Download](workflows/31-RAG_AI_Agent_Very_Accurate.zip) | [🎥 Part 1](https://www.youtube.com/watch?v=nMURjvGpVYo&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=42) • [🎥 Part 2](https://www.youtube.com/watch?v=SWXhl0iQNX4&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=43) |
| **Fully Synchronized RAG Agent** | Enterprise-grade RAG system with real-time data sync, multi-source integration, and advanced caching | [📁 Download](workflows/32-Course32-%20RAG%20AI%20Agent_FullySync.zip) | [🎥 Watch](https://www.youtube.com/watch?v=WtmpNiGbuw4&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=44) |
| **Business Owner GPT-5** | Comprehensive business AI assistant with financial analysis, market research, and strategic planning capabilities | [📁 Download](workflows/41_BusinessOwnerGPT5/41-BOwner_GPT5.json) | [🎥 Watch](https://www.youtube.com/watch?v=LTAw0B5bKb4&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=55&t=451s) |
| **Auto Meeting Scheduler** | Intelligent meeting scheduler with voice and text processing, calendar integration, and conflict resolution | [📁 Download](workflows/42-%20Auto%20Meeting%20Shecduler_Text+Voice.zip) | [🎥 Watch](https://www.youtube.com/watch?v=x0EZioGrEMM&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=55) |
| **Multi-Modal WhatsApp Agent** | Advanced WhatsApp AI agent supporting voice messages, text, images, and document processing with calendar integration | [📁 Download](workflows/46-Voice+Text+Image_WhatsAppAgent.json) | [🎥 Watch](https://www.youtube.com/watch?v=X1m10pipxU8) |
| **WhatsApp Multi-Agent System** | Comprehensive multi-agent WhatsApp system with voice, text, and image processing. Features specialized agents for calendar management, email handling, web search, and social media posting | [📁 Download](workflows/49-WhatsApp%20Multi-Agent_Voice_Text_Image.json) | [🎥 Watch](https://www.youtube.com/watch?v=VSeo-PUuva8&list=PLZ42ZUInDWC79Bw1K_tYQhUPfFRV7fy8v&index=62) |
| **Secured WhatsApp Multi-Agent System** | Enhanced multi-agent WhatsApp system with voice, text, and image processing plus security authentication. Features specialized agents for calendar management, email handling, web search, and social media posting with authorized user verification | [📁 Download](workflows/50_WAgentEnhancement_Secured.json) | [🎥 Watch](https://www.youtube.com/watch?v=VIDEO_ID_PLACEHOLDER) *(Coming Soon)* |

## 🛠️ Technologies & Integrations

### **Core Technologies**
- **n8n** - Visual workflow automation platform
- **OpenAI GPT** - Advanced language models
- **Grok AI** - Alternative AI model for enhanced conversations
- **Vector Databases** - Pinecone, Weaviate for RAG systems
- **Speech Processing** - Voice-to-text and text-to-speech

### **Popular Integrations**
- **📧 Email**: Gmail, Outlook automation
- **📅 Calendar**: Google Calendar, Outlook Calendar
- **💬 Chat**: Slack, Discord, Telegram, WhatsApp
- **🗄️ Database**: PostgreSQL, MySQL, MongoDB
- **☁️ Cloud**: AWS, Google Cloud, Azure services
- **📊 Analytics**: Google Analytics, custom dashboards

## 📖 Learning Path

**Recommended progression through the course:**

1. **🟢 Start Here**: Smart Company Chatbot → Company Chatbot with Grok
2. **🟡 Build Knowledge**: RAG Chatbot → RAG AI Agent → GPT Open Source
3. **🔴 Master Advanced**: High-Accuracy RAG → Fully Synchronized RAG → Business GPT-5
4. **🚀 Capstone**: Auto Meeting Scheduler (combines all skills)

## 🔧 Setup Requirements

### **Required Accounts** (Free tiers available)
- **OpenAI API** - For GPT models
- **Pinecone** - Vector database for RAG systems
- **Google Cloud** - Speech and document processing
- **Telegram/Discord** - For chatbot deployment

### **Optional but Recommended**
- **Grok AI API** - Alternative AI model
- **AWS/Azure** - Cloud deployment
- **MongoDB Atlas** - Document storage
- **Zapier** - Additional integrations

## 🆘 Troubleshooting

### **Common Issues**

**❌ "Workflow won't start"**
- ✅ Check API keys are properly configured
- ✅ Verify webhook URLs are accessible
- ✅ Ensure all required nodes are activated

**❌ "API rate limits exceeded"**
- ✅ Implement delay nodes between API calls
- ✅ Use API key rotation for high-volume workflows
- ✅ Consider upgrading to paid API tiers

**❌ "RAG system not finding relevant documents"**
- ✅ Check document chunking strategy
- ✅ Verify vector embeddings are generated correctly
- ✅ Adjust similarity search thresholds

### **Getting Help**
- **📺 Video Comments** - Ask questions on specific YouTube videos
- **🐛 GitHub Issues** - Report bugs or request features
- **💬 Community** - Join our Discord for real-time help

## 🔗 Additional Resources

### **Official Documentation**
- [📖 n8n Documentation](https://docs.n8n.io/)
- [🤖 OpenAI API Docs](https://platform.openai.com/docs)
- [🗂️ Pinecone Documentation](https://docs.pinecone.io/)

### **Advanced Learning**
- [🎓 n8n Academy](https://academy.n8n.io/)
- [🧠 AI/ML Resources](https://github.com/josephmisiti/awesome-machine-learning)
- [⚡ Automation Best Practices](https://n8n.io/blog/workflow-automation-best-practices/)
- [🚀 Awesome N8N: Top 100 Community Nodes](https://github.com/restyler/awesome-n8n) - Curated list of the most popular community nodes and resources

## 👨‍🏫 About the Instructor

**Mohamed Elshamy** - AI Automation Expert & Course Creator

Specialized in building production-ready AI agents and automation systems. Creator of multiple successful courses on YouTube with thousands of students worldwide.

- **🎥 YouTube Channel**: [AI Plus ME](https://www.youtube.com/@AIPlus_ME)
- **💼 LinkedIn**: [Mohamed Elshamy](https://www.linkedin.com/in/moh-elshamy/)
- **🐙 GitHub**: [MohElshamy1994](https://github.com/MohElshamy1994)

## 🤝 Contributing

Found a bug or want to improve a workflow? Contributions are welcome!

1. **Fork this repository**
2. **Create your feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

## 📜 License

This educational content is provided for learning purposes. Please respect the terms of service of all integrated platforms and APIs.

---

**⭐ If you find this course helpful, please star this repository and subscribe to the YouTube channel!**

**🚀 Ready to build intelligent AI agents? Start with the Quick Start Guide above!**

**Happy Automating! 🤖✨**