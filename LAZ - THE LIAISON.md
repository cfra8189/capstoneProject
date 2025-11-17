# L|A|Z - THE LIAISON
**Hospital Communication Request Management System**

---

## **EXECUTIVE SUMMARY**

L|A|Z (Link/Liaison) is a digital request management system designed to streamline communication between nursing staff and nutrition services in hospital settings. Based on real-world experience as a Diet Office Liaison at WellStar Hospital, this application addresses inefficiencies in the current manual request process, reducing delays and improving patient care.

---

## **PROBLEM STATEMENT**

### **Current Process Issues:**
- Nurses submit nutrition requests via phone calls or paper tickets
- No centralized tracking system for request status
- Frequent delays in communication between departments
- Lost or incomplete requests lead to patient dissatisfaction
- No accountability or audit trail for request fulfillment
- Dietary changes require manual approval coordination with dietitians

### **Impact:**
- Decreased patient satisfaction due to delayed or incorrect meal service
- Increased staff frustration and inefficiency
- Time wasted on follow-up calls and duplicate requests
- Risk of dietary errors affecting patient health

---

## **SOLUTION OVERVIEW**

L|A|Z provides a centralized digital platform where nursing staff can:
- Search and select patients quickly
- Submit multiple request types in a single workflow
- Track request status in real-time
- Coordinate approvals with dietitians electronically
- Generate printable documentation
- Monitor request completion times

---

## **USER STORIES**

### **Primary Users:**

**Nurses:**
- "As a nurse, I want to quickly request a meal tray for a transfer patient so they receive timely nutrition."
- "As a nurse, I want to change a patient's supplement preference so they receive something they'll actually consume."
- "As a nurse, I want to see the status of my requests so I can follow up if needed."

**Liaisons (Diet Office Staff):**
- "As a liaison, I want to see all pending requests with time tracking so I can prioritize urgent needs."
- "As a liaison, I want to mark requests complete so nursing knows the task is done."

**Dietitians:**
- "As a dietitian, I want to receive notifications for supplement change requests so I can approve them based on the patient's medical needs."

---

## **CORE FEATURES (MVP)**

### **1. Patient Search**
Search patients by:
- First and Last Name
- Room Number
- Medical Record Number (MRN)

Display patient information including:
- Demographics (name, DOB, room, unit)
- Current diet order
- Allergies
- Current supplements
- Assigned healthcare team
- Insulin status
- Risk level

### **2. Request Types**

#### **A. Tray Request**
- Pre-populated with patient's current diet order
- Ability to edit meal details
- Select meal type (breakfast, lunch, dinner, snack)
- Add special instructions

#### **B. Supplement Change Request**
- Select current supplement to change/remove
- Choose preferred replacement supplement
- Add reason/notes
- Triggers approval workflow for dietitian

#### **C. Liaison Request**
- Contact liaison directly through app
- Dropdown menu for request category:
  - Menu options inquiry
  - Tray notification
  - Other (with text field)
- Custom message field

#### **D. Report Missing Tray/Items**
- Report missing meal delivery
- Document missing items from delivered tray
- Time-stamped for tracking

### **3. Multi-Request Submission**
- Create multiple requests before submitting
- Review all requests together
- Submit as batch
- Generate printable summary document

### **4. Request Tracking Dashboard**
- Display all active requests
- Real-time timer showing how long request has been pending
- Status indicators:
  - Pending
  - Awaiting Approval
  - In Progress
  - Complete
- Filter by unit, request type, or time range

### **5. Approval Workflow**
- Email notification to dietitian for supplement changes
- Dietitian reviews patient chart and approves/denies
- Status updates reflected in real-time
- Audit trail of who approved and when

### **6. Completion Tracking**
- Mark individual requests as complete
- Timer stops when completed
- "Upon Approval" indicator for partially completed orders
- Historical record of completion times

---

## **TECHNICAL ARCHITECTURE**

### **Frontend:**
- **React** with **TypeScript**
- **Tailwind CSS** for styling
- **React Hook Form** for form management
- **React Router** for navigation
- Responsive design for desktop and tablet use

### **Backend:**
- **Node.js** with **Express**
- RESTful API architecture
- **JWT** for authentication
- Role-based access control

### **Database:**
- **PostgreSQL** (relational database)
- **Prisma** ORM for type-safe database access

### **Additional Services:**
- **Nodemailer** for email notifications
- **jsPDF** or **React-to-PDF** for printable documents
- **Day.js** for time tracking and date handling

### **Deployment:**
- **Frontend:** Vercel or Netlify
- **Backend:** Render or Railway
- **Database:** Render PostgreSQL or Supabase
- Live demo URL for portfolio

---

## **DATA MODEL**

### **Core Entities:**
```typescript
interface Patient {
  id: string;
  mrn: string; // Medical Record Number (unique)
  firstName: string;
  lastName: string;
  dob: Date;
  unit: string;
  roomNumber: string;
  bedNumber: number;
  dietOrder: string;
  allergies: string[];
  supplements: string[];
  dieticianId: string;
  physicianId: string;
  nurseId: string;
  carepartnerId: string;
  admissionDate: Date;
  onInsulin: boolean;
  riskOfViolenceLevel: number;
}

interface User {
  id: string;
  name: string;
  email: string;
  role: 'nurse' | 'liaison' | 'dietitian' | 'admin';
  employeeId: string;
  unit?: string;
  profilePicture?: string;
  onBreak?: boolean;
}

interface Request {
  id: string;
  patientId: string;
  requestedById: string;
  type: 'tray' | 'supplement_change' | 'liaison' | 'missing_items';
  status: 'pending' | 'approved' | 'completed' | 'cancelled';
  createdAt: Date;
  completedAt?: Date;
  notes?: string;
  requiresApproval: boolean;
  approvedById?: string;
  approvedAt?: Date;
}

interface TrayRequest {
  id: string;
  requestId: string;
  mealType: 'breakfast' | 'lunch' | 'dinner' | 'snack';
  dietOrder: string;
  customizations?: string;
}

interface SupplementChangeRequest {
  id: string;
  requestId: string;
  currentSupplement: string;
  requestedSupplement: string;
  reason?: string;
}

interface LiaisonRequest {
  id: string;
  requestId: string;
  liaisonId: string;
  category: 'menu_option' | 'tray_notification' | 'other';
  message: string;
}
```

---

## **USER WORKFLOW**

### **Scenario: Nurse Requesting Tray and Supplement Change**

1. **Login** to application with hospital credentials
2. **Search Patient** using name, room number, or MRN
3. **Select Patient** from search results
4. **View Patient Information** to confirm correct patient and review diet details
5. **Create Tray Request:**
   - Review pre-populated diet information
   - Select meal type (e.g., dinner)
   - Add any special instructions
   - Save request
6. **Create Supplement Change Request:**
   - Select current supplement (e.g., Chocolate Ensure Max)
   - Choose preferred supplement (e.g., Vanilla Ensure Max)
   - Add reason for change
   - Save request
7. **Review All Requests** in selection window
8. **Submit All Requests** together
9. **Print Confirmation** if needed
10. **Monitor Status** on dashboard

### **Liaison Workflow:**

1. **View Dashboard** showing all pending requests
2. **See Real-Time Timers** for each request
3. **Process Requests** in priority order
4. **Mark Complete** when fulfilled
5. **Monitor Approval-Pending Requests**

### **Dietitian Workflow:**

1. **Receive Email Notification** for supplement change request
2. **Review Patient Information** in email
3. **Access Patient Chart** (external system)
4. **Approve or Deny** request via email link or app
5. **System Updates Status** automatically

---

## **UI/UX DESIGN PRINCIPLES**

### **Design Goals:**
- Clean, uncluttered interface (healthcare staff are busy)
- Large touch targets (tablet-friendly)
- Clear visual hierarchy
- Minimal clicks to complete common tasks
- Status indicators with color coding
- Accessible (WCAG 2.1 AA compliance)

### **Color Scheme:**
- **Primary:** Soft blue or teal (healthcare trust)
- **Success:** Muted green
- **Warning:** Soft amber
- **Error:** Muted red
- **Neutral:** Grays, whites
- Good contrast for readability

### **Typography:**
- Clean sans-serif (Inter, Roboto, or system fonts)
- Clear hierarchy through size and weight
- Readable at various screen sizes

### **Layout:**
- Consistent navigation
- Breadcrumbs for multi-step workflows
- Form validation with helpful error messages
- Loading states for async operations

---

## **SECURITY & COMPLIANCE CONSIDERATIONS**

### **For MVP/Capstone:**
- User authentication (JWT tokens)
- Role-based access control
- Input validation and sanitization
- Secure password hashing
- HTTPS for all communications

### **For Production (Future):**
- **HIPAA Compliance:**
  - Encrypted data at rest and in transit
  - Audit logs for all patient data access
  - Business Associate Agreements (BAAs)
  - Regular security audits
- **Integration with Hospital Systems:**
  - Single Sign-On (SSO) with hospital Active Directory
  - EMR/EHR system integration
  - HL7 or FHIR standards compliance

---

## **DEVELOPMENT TIMELINE**

### **Week 1: Foundation**
- Set up project repositories (frontend/backend)
- Initialize database with schema
- Create seed data (sample patients, users)
- Basic API endpoints (CRUD for patients)
- Frontend project structure and routing

### **Week 2: Core Features**
- Patient search functionality
- Display patient information page
- Build all form components
- Form validation logic

### **Week 3: Request Management**
- Request submission logic
- Backend request processing
- Request tracking dashboard
- Timer implementation
- Basic approval workflow

### **Week 4: Polish & Testing**
- UI/UX refinement and styling
- Responsive design implementation
- End-to-end testing
- Bug fixes
- Documentation

### **Week 5-6 (If Available):**
- Email notifications
- PDF generation
- Additional features
- Deployment
- Presentation preparation

---

## **SUCCESS METRICS**

### **Technical:**
- All core features functional
- Responsive design works on desktop and tablet
- Page load times under 2 seconds
- No critical bugs in demo
- Clean, documented code
- Deployed with live URL

### **User-Focused:**
- Complete workflow from search to submission in under 2 minutes
- Clear visual feedback at every step
- Intuitive navigation (minimal training needed)
- Request status visible at a glance

---

## **FUTURE ENHANCEMENTS (Post-Capstone)**

### **Phase 2 Features:**
- Mobile app (React Native)
- Real-time updates (WebSockets)
- Advanced analytics dashboard
- Dietary inventory management
- Integration with hospital paging systems
- Multi-language support

### **Phase 3 Features:**
- EMR/EHR integration
- Automated meal ordering based on diet plans
- Patient portal (patients can view menu and place orders)
- Predictive analytics for request volume
- Staff scheduling integration

---

## **MARKET POTENTIAL**

### **Target Market:**
- 6,090 hospitals in the United States
- Every hospital has nutrition services
- Current solutions often outdated or non-existent

### **Competitive Advantage:**
- Built by someone with firsthand experience in the role
- Focused specifically on nutrition communication (not generic ticketing)
- Simple, intuitive interface designed for busy healthcare staff
- Addresses actual pain points observed in daily operations

### **Business Model (If Commercialized):**
- SaaS subscription: $200-$500/month per hospital
- Implementation/training services
- Custom integrations for enterprise clients

---

## **RISKS & MITIGATION**

### **Technical Risks:**
- **Risk:** Complexity of approval workflows
  - **Mitigation:** Start with simple approval flow, iterate based on feedback

- **Risk:** Real-time updates performance
  - **Mitigation:** Use polling for MVP, implement WebSockets later

- **Risk:** Data security concerns
  - **Mitigation:** Follow security best practices, use established authentication libraries

### **Scope Risks:**
- **Risk:** Feature creep
  - **Mitigation:** Strict prioritization, focus on MVP features only

- **Risk:** Timeline overruns
  - **Mitigation:** Weekly progress check-ins, adjust scope if needed

---

## **TESTING STRATEGY**

### **Unit Tests:**
- API endpoint functionality
- Form validation logic
- Data model methods

### **Integration Tests:**
- End-to-end request workflows
- Database operations
- Authentication flow

### **User Acceptance Testing:**
- Demo to current coworkers at WellStar
- Gather feedback on usability
- Iterate on pain points

---

## **PRESENTATION PLAN**

### **Demo Flow (5-7 minutes):**

1. **Introduction (30 sec):**
   - "As a Liaison at WellStar Hospital, I process dozens of nutrition requests daily..."
   - Problem statement: delays, lost requests, no tracking

2. **Live Demo (3-4 min):**
   - Login as nurse
   - Search for patient
   - Create tray request
   - Create supplement change request
   - Submit requests
   - Show liaison dashboard with tracking
   - Mark request complete

3. **Technical Overview (1 min):**
   - Tech stack
   - Database design
   - Key technical challenges solved

4. **Impact & Future (1 min):**
   - Measurable improvements (time saved, reduced errors)
   - Potential for hospital-wide implementation
   - Future feature roadmap

5. **Q&A**

---

## **DOCUMENTATION DELIVERABLES**

- **README.md:** Project overview, setup instructions, tech stack
- **API Documentation:** Endpoint descriptions, request/response examples
- **Database Schema:** ERD diagram, table descriptions
- **User Guide:** How to use the application (with screenshots)
- **Developer Guide:** How to run locally, deployment process
- **Presentation Slides:** For capstone demo day

---

## **PERSONAL MOTIVATION**

This project combines my real-world healthcare experience with software engineering training to solve a problem I encounter daily. Working as a Liaison has given me unique insight into the communication challenges between nursing staff and nutrition services. I've seen firsthand how technology could significantly improve patient care and staff efficiency. Building L|A|Z allows me to create a tangible solution that could benefit not just WellStar, but hospitals nationwide.

---

## **CONCLUSION**

L|A|Z addresses a real problem in healthcare communication with a focused, user-centered solution. By leveraging my domain expertise as a hospital liaison and applying full-stack development skills learned at Per Scholas, this capstone project demonstrates both technical competency and practical problem-solving ability. The MVP scope is achievable within the capstone timeline while providing a strong foundation for future development and potential commercialization.

---

**Project Repository:** [To be created]  
**Live Demo:** [To be deployed]  
**Contact:** Clarence Franklin | cfra8189@gmail.com | 318-500-4318

---

**END OF DOCUMENT**