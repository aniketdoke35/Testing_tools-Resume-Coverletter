Template Developer Guide
This guide provides comprehensive instructions for creating new resume templates using the same architecture and functionality as Template.html.

Overview
Template is a modern, responsive resume template with the following key features:

PostMessage communication for data exchange
Theme customization support
Drag and drop functionality
Responsive design for all devices
Print optimization
Comprehensive section support
Template Structure
Basic HTML Structure

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Resume Template</title>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
    />
  </head>
  <style>
    /* CSS Styles */
  </style>
  <body>
    <!-- HTML Content -->
    <script>
      /* JavaScript Functions */
    </script>
  </body>
</html>
Required HTML Elements
Every template must include these essential elements with specific IDs:

<!-- Header Section -->
<div class="resume-header">
  <div id="profile-img-container" class="profile-img-container">
    <img id="profile-img" src="" alt="Profile Picture" style="display: none;" />
  </div>
  <div class="only-contact">
    <h1 id="full-name" class="name">Name</h1>
    <h2 id="professional-title" class="title">Professional Title</h2>
    <div class="contact">
      <div>
        <span id="phone" class="contact-item"> <i class="fas fa-phone"></i> Phone </span>
        <span id="email" class="contact-item"> <i class="fas fa-envelope"></i> Email </span>
      </div>
      <div>
        <span id="linkedin" class="contact-item" style="display: none;">
          <i class="fab fa-linkedin"></i> LinkedIn
        </span>
        <span id="website-container" class="contact-item">
          <i class="fas fa-map-marker-alt"></i> Location
        </span>
        <span id="gender" class="contact-item" style="display: none;">
          <i class="fas fa-venus-mars"></i> Gender
        </span>
      </div>
    </div>
  </div>
</div>

<!-- Required Sections -->
<div id="professional-summary-section" class="section">
  <h2 class="section-title">Profile</h2>
  <p id="professional-summary">Summary</p>
</div>

<div id="work-experience-section" class="section">
  <h2 class="section-title">Experience</h2>
  <div id="work-experience-container"></div>
</div>

<div id="education-section" class="section">
  <h2 class="section-title">Education</h2>
  <div id="education-container"></div>
</div>

<div id="skills-section" class="section">
  <h2 class="section-title">Skills</h2>
  <div id="skills-container" class="skills-grid"></div>
</div>

<!-- Additional Sections (use display: none initially) -->
<div id="internships-section" class="section" style="display: none;">
  <h2 class="section-title">Internships</h2>
  <div id="internships-container"></div>
</div>

<div id="projects-section" class="section" style="display: none;">
  <h2 class="section-title">Projects</h2>
  <div id="projects-container"></div>
</div>

<div id="certifications-section" class="section" style="display: none;">
  <h2 class="section-title">Certifications</h2>
  <div id="certifications-container"></div>
</div>

<div id="affiliations-section" class="section" style="display: none;">
  <h2 class="section-title">Affiliations</h2>
  <div id="affiliations-container"></div>
</div>

<div id="references-section" class="section" style="display: none;">
  <h2 class="section-title">References</h2>
  <div id="references-container"></div>
</div>

<div id="interests-section" class="section" style="display: none;">
  <h2 class="section-title">Interests</h2>
  <div id="interests-container"></div>
</div>

<div id="accomplishments-section" class="section" style="display: none;">
  <h2 class="section-title">Accomplishments</h2>
  <div id="accomplishments-container"></div>
</div>

<div id="strengths-section" class="section" style="display: none;">
  <h2 class="section-title">Strengths</h2>
  <div id="strengths-container"></div>
</div>

<div id="publications-section" class="section" style="display: none;">
  <h2 class="section-title">Publications</h2>
  <div id="publications-container"></div>
</div>

<div id="volunteering-section" class="section" style="display: none;">
  <h2 class="section-title">Volunteering</h2>
  <div id="volunteering-container"></div>
</div>

<div id="languages-section" class="section" style="display: none;">
  <h2 class="section-title">Languages</h2>
  <div id="languages-container"></div>
</div>

<div id="socialMedia-section" class="section" style="display: none;">
  <h2 class="section-title">Social Media</h2>
  <div id="socialMedia-container"></div>
</div>

<div id="summaries-section" class="section" style="display: none;">
  <h2 class="section-title">Summaries</h2>
  <div id="summaries-container"></div>
</div>

<!-- Custom Sections Container -->
<div id="custom-sections-container"></div>
Required JavaScript Functions
1. PostMessage Event Listener
// Listen for messages from parent window
window.addEventListener('message', function (event) {
  if (event.data && event.data.type === 'RESUME_DATA') {
    renderResumeData(event.data.payload);
  } else if (event.data && event.data.type === 'CHANGE_THEME') {
    applyTheme(event.data.payload);
  }
});
2. Main Data Rendering Function
function renderResumeData(data) {
  if (!data) {
    return;
  }

// Check if we're in edit mode
const isEditable = data.isEditable || false;
document.body.dataset.editable = isEditable;

if (!data.sections) {
data.sections = {};
}

// Move common data into sections for consistent rendering
if (data.skills && !data.sections.skills) {
data.sections.skills = data.skills;
}
// ... (similar mappings for all sections)

if (!data.personalInfo) {
data.personalInfo = {};
}

if (data.contact) {
data.personalInfo = { ...data.personalInfo, ...data.contact };
}

// Profile Image handling
(function renderProfileImage() {
const imgContainer = document.getElementById('profile-img-container');
const imgEl = document.getElementById('profile-img');
const imgData =
data.personalInfo.profileImage || data.personalInfo.photo || data.personalInfo.avatar;
if (!imgEl) return;
// ... (image handling logic)
})();

// Personal Info
if (data.personalInfo.firstName || data.personalInfo.lastName) {
document.getElementById('full-name').textContent = `${data.personalInfo.firstName || ''} ${
      data.personalInfo.lastName || ''
    }`.trim();
}

if (data.personalInfo.professionalTitle || data.personalInfo.jobTitle) {
document.getElementById('professional-title').textContent =
data.personalInfo.professionalTitle || data.personalInfo.jobTitle;
}

// Professional Summary
const summaryText = data.personalInfo.profileDescription;
if (summaryText) {
document.getElementById('professional-summary').textContent = summaryText;
const summarySection = document.getElementById('professional-summary-section');
if (summarySection) {
summarySection.style.display = 'block';
}
} else {
const summarySection = document.getElementById('professional-summary-section');
if (summarySection) {
summarySection.style.display = 'none';
}
}

// Contact Info
if (data.personalInfo.email) {
document.getElementById(
'email'
).innerHTML = `<i class="fa-solid fa-envelope"></i> ${data.personalInfo.email}`;
document.getElementById('email').style.display = 'block';
}

if (data.personalInfo.phone) {
document.getElementById(
'phone'
).innerHTML = `<i class="fa-solid fa-phone"></i> ${data.personalInfo.phone}`;
document.getElementById('phone').style.display = 'block';
}

if (data.personalInfo.linkedin) {
document.getElementById(
'linkedin'
).innerHTML = `<i class="fa-brands fa-linkedin"></i> ${data.personalInfo.linkedin}`;
document.getElementById('linkedin').style.display = 'block';
}

// Update location
const location = [data.personalInfo.city, data.personalInfo.state, data.personalInfo.country]
.filter(Boolean)
.join(', ');
if (location) {
document.getElementById(
'website-container'
).innerHTML = `<i class="fa-solid fa-map-marker-alt"></i> ${location}`;
}

// Add gender info
if (data.personalInfo.gender) {
const genderElement = document.getElementById('gender');
if (genderElement) {
genderElement.innerHTML = `<i class="fa-solid fa-venus-mars"></i> ${data.personalInfo.gender}`;
genderElement.style.display = 'block';
}
}

// Render all sections
renderSections(data.sections);

// Handle draggable attributes based on editable state
const sections = document.querySelectorAll('.section');
sections.forEach((section) => {
section.setAttribute('data-editable', isEditable);
if (isEditable) {
if (!section.classList.contains('draggable')) {
section.classList.add('draggable');
}
if (!section.hasAttribute('draggable')) {
section.setAttribute('draggable', 'true');
}
} else {
section.classList.remove('draggable');
if (section.hasAttribute('draggable')) {
section.removeAttribute('draggable');
}
}
});

// Initialize drag and drop after rendering if editable
if (isEditable) {
setTimeout(() => {
initializeDragAndDrop();
}, 100);
}
} 3. Section Rendering Functions
Each section requires a dedicated rendering function:

// Function to dynamically render all sections
function renderSections(sections) {
for (const [sectionKey, sectionData] of Object.entries(sections)) {
switch (sectionKey) {
case 'skills':
renderSkills(sectionData);
break;
case 'workExperience':
renderWorkExperience(sectionData);
break;
case 'education':
renderEducation(sectionData);
break;
case 'projects':
renderProjects(sectionData);
break;
case 'certifications':
renderCertifications(sectionData);
break;
case 'languages':
renderLanguages(sectionData);
break;
case 'customSections':
renderCustomSections(sectionData);
break;
case 'accomplishments':
renderAccomplishments(sectionData);
break;
case 'interests':
renderInterests(sectionData);
break;
case 'affiliations':
renderAffiliations(sectionData);
break;
case 'publications':
renderPublications(sectionData);
break;
case 'references':
renderReferences(sectionData);
break;
case 'socialMedia':
renderSocialMedia(sectionData);
break;
case 'strengths':
renderStrengths(sectionData);
break;
case 'volunteering':
renderVolunteering(sectionData);
break;
case 'internships':
renderInternships(sectionData);
break;
case 'summaries':
renderSummaries(sectionData);
break;
default:
break;
}
}

hideEmptySections(sections);
} 4. Individual Section Renderers
Example for work experience:

function renderWorkExperience(experiences) {
const workExperienceContainer = document.getElementById('work-experience-container');
workExperienceContainer.innerHTML = '';

if (experiences && experiences.length > 0) {
experiences.forEach((exp) => {
const experienceItem = document.createElement('div');
experienceItem.className = 'experience-item';

      // Header (Title + Date)
      const headerDiv = document.createElement('div');
      headerDiv.style.overflow = 'hidden';
      headerDiv.style.marginBottom = '5px';

      const experienceTitle = document.createElement('div');
      experienceTitle.className = 'experience-title';
      experienceTitle.style.float = 'left';
      experienceTitle.textContent = exp.position || exp.jobTitle || exp.title || '';

      const experienceDate = document.createElement('div');
      experienceDate.className = 'datas';
      const isCurrent = exp.isCurrent || exp.is_current || exp.current || false;
      const start = exp.startDate || exp.start_date;
      const end = exp.endDate || exp.end_date;
      experienceDate.textContent = getDateRange(start, end, isCurrent);

      headerDiv.appendChild(experienceTitle);
      headerDiv.appendChild(experienceDate);

      // Company + Location
      const experienceCompany = document.createElement('div');
      experienceCompany.className = 'experience-company';
      const loc = exp.location || [exp.city, exp.countryName].filter(Boolean).join(', ');
      experienceCompany.textContent = `${exp.company || ''}${loc ? ` - ${loc}` : ''}`;

      // Description
      const experienceDescription = document.createElement('div');
      experienceDescription.className = 'experience-description';
      if (exp.description) {
        experienceDescription.textContent = exp.description.trim();
      }

      // Append all parts
      experienceItem.appendChild(headerDiv);
      experienceItem.appendChild(experienceCompany);
      experienceItem.appendChild(experienceDescription);

      workExperienceContainer.appendChild(experienceItem);
    });

}
} 5. Helper Functions
function formatDate(dateString, format = 'medium') {
if (!dateString) return '';
const date = new Date(dateString);
if (isNaN(date.getTime())) {
return dateString;
}
switch (format) {
case 'short':
return `${date.getMonth() + 1}/${date.getFullYear()}`;
case 'long':
return date.toLocaleDateString('en-US', { year: 'numeric', month: 'long' });
case 'medium':
default:
return date.toLocaleDateString('en-US', { year: 'numeric', month: 'short' });
}
}

function getDateRange(startDate, endDate, isCurrent = false, format = 'medium') {
const start = formatDate(startDate, format);
if (isCurrent) {
return `${start} - Present`;
}
const end = formatDate(endDate, format);
return `${start} - ${end}`;
}

function hideEmptySections(sections) {
const sectionIds = {
skills: 'skills-section',
workExperience: 'work-experience-section',
education: 'education-section',
projects: 'projects-section',
certifications: 'certifications-section',
languages: 'languages-section',
accomplishments: 'accomplishments-section',
interests: 'interests-section',
affiliations: 'affiliations-section',
publications: 'publications-section',
references: 'references-section',
socialMedia: 'socialMedia-section',
strengths: 'strengths-section',
volunteering: 'volunteering-section',
internships: 'internships-section',
summaries: 'summaries-section',
};

// Handle professional summary section separately
const professionalSummarySection = document.getElementById('professional-summary-section');
if (professionalSummarySection) {
const summaryText = document.getElementById('professional-summary').textContent;
if (!summaryText || summaryText.trim() === '') {
professionalSummarySection.style.display = 'none';
} else {
professionalSummarySection.style.display = 'block';
}
}

for (const [sectionKey, sectionId] of Object.entries(sectionIds)) {
const sectionElement = document.getElementById(sectionId);
if (!sectionElement) {
continue;
}

    const sectionData = sections[sectionKey];
    let isEmpty = false;

    if (!sectionData) {
      isEmpty = true;
    } else if (Array.isArray(sectionData)) {
      isEmpty =
        sectionData.length === 0 ||
        sectionData.every((item) => !item || Object.keys(item).length === 0);
    } else if (typeof sectionData === 'object') {
      isEmpty = Object.keys(sectionData).length === 0;
    }

    // Also check if the main content container is empty after rendering
    let container =
      sectionElement.querySelector('#' + sectionId.replace('-section', '') + '-container') ||
      sectionElement.querySelector('[id$="-container"]');

    // Special cases
    if (sectionKey === 'strengths' && !container) {
      container = document.getElementById('strengths-container');
    }

    if (sectionKey === 'strengths' && sectionData && sectionData.length > 0) {
      isEmpty = false;
    } else if (sectionKey === 'affiliations' && sectionData && sectionData.length > 0) {
      isEmpty = false;
    } else if (sectionKey === 'interests' && sectionData && sectionData.length > 0) {
      isEmpty = false;
    } else if (container && container.innerHTML.trim() === '') {
      isEmpty = true;
    }

    if (isEmpty) {
      sectionElement.style.display = 'none';
    } else {
      sectionElement.style.display = 'block';
    }

}
} 6. Drag and Drop Functions
// Drag and drop functionality
let draggedElement = null;

function handleDragStart(e) {
const isEditable = document.body.dataset.editable === 'true';
if (!isEditable) {
e.preventDefault();
return false;
}

draggedElement = this;
this.classList.add('being-dragged');
e.dataTransfer.effectAllowed = 'move';
e.dataTransfer.setData('text/html', this.innerHTML);
}

function handleDragOver(e) {
e.preventDefault();
const isEditable = document.body.dataset.editable === 'true';
if (!isEditable) {
return;
}

if (this !== draggedElement) {
this.classList.add('drag-over');
e.dataTransfer.dropEffect = 'move';
}
}

function handleDragLeave(e) {
this.classList.remove('drag-over');
}

function handleDrop(e) {
e.preventDefault();
this.classList.remove('drag-over');

const isEditable = document.body.dataset.editable === 'true';
if (!isEditable || !draggedElement) {
return;
}

if (draggedElement !== this) {
const parent = this.parentNode;
if (parent) {
if (draggedElement.parentNode === parent) {
const allChildren = Array.from(parent.children);
const draggedIndex = allChildren.indexOf(draggedElement);
const targetIndex = allChildren.indexOf(this);

        if (draggedIndex < targetIndex) {
          parent.insertBefore(draggedElement, this.nextSibling);
        } else {
          parent.insertBefore(draggedElement, this);
        }
      }
    }

}
}

function handleDragEnd(e) {
document.querySelectorAll('.section').forEach((section) => {
section.classList.remove('being-dragged', 'drag-over');
});
draggedElement = null;
}

function initializeDragAndDrop() {
const isEditable = document.body.dataset.editable === 'true';
if (!isEditable) {
return;
}

const allSections = document.querySelectorAll('.section');
const draggableSections = Array.from(allSections).filter((section) => {
return section.hasAttribute('draggable') && section.getAttribute('draggable') === 'true';
});

if (draggableSections.length === 0) {
allSections.forEach((section) => {
section.setAttribute('data-editable', isEditable);
if (isEditable) {
if (!section.hasAttribute('draggable')) {
section.setAttribute('draggable', 'true');
}
if (!section.classList.contains('draggable')) {
section.classList.add('draggable');
}
}
});
const updatedSections = document.querySelectorAll('.section[draggable="true"]');
draggableSections.push(...Array.from(updatedSections));
}

draggableSections.forEach((section) => {
section.removeEventListener('dragstart', handleDragStart);
section.removeEventListener('dragover', handleDragOver);
section.removeEventListener('dragleave', handleDragLeave);
section.removeEventListener('drop', handleDrop);
section.removeEventListener('dragend', handleDragEnd);

    if (isEditable) {
      section.addEventListener('dragstart', handleDragStart);
      section.addEventListener('dragover', handleDragOver);
      section.addEventListener('dragleave', handleDragLeave);
      section.addEventListener('drop', handleDrop);
      section.addEventListener('dragend', handleDragEnd);
    }

});
}
CSS Requirements

1. Required CSS Classes
   /_ Base Styles _/
   body {
   font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
   font-size: 14px;
   color: #333;
   margin: 0;
   padding: 0;
   }

.container {
margin: 0 auto;
padding: 40px;
gap: 40px;
}

.resume-header {
display: flex;
justify-content: space-around;
align-items: center;
gap: 20px;
}

.profile-img-container {
width: 200px;
height: 200px;
margin: 20px auto 20px;
border-radius: 50%;
overflow: hidden;
box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
position: relative;
}

.profile-img-container img {
width: 100%;
height: 100%;
object-fit: cover;
border-radius: 50%;
}

.name {
font-size: 28px;
font-weight: bold;
margin: 0;
text-transform: uppercase;
letter-spacing: 2px;
}

.title {
font-size: 16px;
font-weight: 500;
color: #666;
margin: 5px 0 20px;
}

.contact-item {
display: flex;
align-items: center;
font-size: 13px;
margin: 4px 0;
color: #444;
}

.contact-item i {
margin-right: 6px;
color: #000;
width: 16px;
flex-shrink: 0;
}

.section-title {
font-size: 15px;
font-weight: bold;
text-transform: uppercase;
border-bottom: 2px solid #000;
padding-bottom: 4px;
margin-bottom: 10px;
letter-spacing: 1px;
}

.section {
margin-bottom: 25px;
}

/_ Print Styles _/
@media print {
html,
body {
margin: 0;
padding: 0;
height: auto !important;
overflow: visible !important;
font-size: 13px;
}

.print-wrapper {
padding-top: 0;
padding-bottom: 0;
-webkit-box-decoration-break: clone;
box-decoration-break: clone;
display: block;
}

.experience-item,
.education-item,
.project-item,
.skill-item,
.certification-item,
.achievement-item,
.language-item,
.interest-item {
page-break-inside: avoid;
break-inside: avoid;
}

.section-title {
page-break-after: avoid;
break-after: avoid;
font-size: 14px;
}

.section:empty {
display: none;
}
} 2. Section-Specific Styles
Each section should have appropriate styling for its content:

/_ Skills Grid _/
.skills-grid {
display: grid;
grid-template-columns: repeat(4, 1fr);
grid-template-rows: repeat(4, auto);
gap: 10px;
grid-auto-rows: minmax(100px, auto);
}

.skills-grid span {
background: #f2f2f2;
padding: 6px 10px;
border-radius: 4px;
font-size: 12px;
}

.skill-header {
display: flex;
justify-content: space-between;
margin-bottom: 5px;
gap: 7px;
}

.skill-name {
font-size: 13px;
font-weight: bold;
color: #333;
}

.skill-percentage {
font-size: 13px;
color: #666;
}

/_ Work Experience / Education Entries _/
#work-experience-container > div,
#education-container > div,
#publications-container > div,
#certifications-container > div,
#volunteering-container > div {
margin-bottom: 15px;
}

#work-experience-container h3,
#education-container h3 {
font-size: 14px;
font-weight: bold;
margin: 0 0 4px;
}

.datas {
float: right;
}

.experience-title {
font-weight: bold;
}

#work-experience-container span,
#education-container span {
font-size: 12px;
color: #777;
}

/_ Publications, Certifications, Volunteering _/
#publications-container p,
#certifications-container p,
#volunteering-container p {
margin: 0 0 6px;
font-size: 13px;
}
Data Structure Support
Personal Information Fields
Your template must support all these personalInfo fields:

data.personalInfo = {
fullName: 'John Doe',
firstName: 'John',
lastName: 'Doe',
professionalTitle: 'Full Stack Developer',
email: 'john@example.com',
phone: '+1 234 567 8900',
countryCode: 'US',
dateOfBirth: '1990-01-01',
gender: 'Male',
nationality: 'American',
country: 'United States',
state: 'California',
city: 'San Francisco',
streetNumber: '123',
address: '123 Main St',
postalZipCode: '94105',
profileImage: 'data:image/jpeg;base64,...',
profileDescription: 'Experienced developer...',
website: 'https://johndoe.com',
linkedin: 'https://linkedin.com/in/johndoe',
githubLink: 'https://github.com/johndoe',
drivingLicense: 'D12345678',
passportNumber: 'P12345678',
maritalStatus: 'Single',
militaryService: 'None',
};
Section Data Structures
Each section has a specific data structure:

// Work Experience
data.sections.workExperience = [
{
id: 'exp-1',
company: 'Tech Corp',
position: 'Senior Developer',
city: 'San Francisco',
country: 'USA',
startDate: '2020-01-01',
endDate: '2023-01-01',
isCurrent: false,
description: 'Developed web applications...',
},
];

// Education
data.sections.education = [
{
id: 'edu-1',
degree: 'Bachelor of Science',
university: 'University of California',
city: 'Berkeley',
country: 'USA',
startYear: '2015',
endYear: '2019',
description: 'Major in Computer Science',
},
];

// Skills
data.sections.skills = [
{
skillName: 'JavaScript',
proficiencyLevel: 8,
},
];

// Projects
data.sections.projects = [
{
id: 'proj-1',
name: 'E-commerce Platform',
description: 'Built a full-stack e-commerce solution',
projectUrl: 'https://github.com/user/project',
startDate: '2022-01-01',
endDate: '2022-12-01',
isCurrent: false,
},
];

// Certifications
data.sections.certifications = [
{
id: 'cert-1',
name: 'AWS Certified Developer',
authority: 'Amazon Web Services',
certificationDate: '2023-01-01',
certificationDescription: 'Certified in AWS development',
certificationUrl: 'https://aws.com/certification',
},
];

// Languages
data.sections.languages = [
{
id: 'lang-1',
name: 'English',
proficiencyLevel: 'Fluent',
canRead: true,
canWrite: true,
canSpeak: true,
},
];

// Accomplishments
data.sections.accomplishments = [
{
id: 'acc-1',
title: 'Employee of the Year',
description: 'Recognized for outstanding performance',
organization: 'Tech Corp',
date: '2022-12-01',
category: 'Award',
},
];

// Interests
data.sections.interests = [
{
id: 'int-1',
name: 'Photography',
category: 'Creative',
description: 'Landscape and portrait photography',
},
];

// Affiliations
data.sections.affiliations = [
{
id: 'aff-1',
organization: 'IEEE Member',
role: 'Member',
startDate: '2020-01-01',
endDate: '2023-01-01',
description: 'Active member in technical community',
},
];

// Publications
data.sections.publications = [
{
id: 'pub-1',
title: 'Modern Web Development Techniques',
publisher: 'Tech Journal',
date: '2023-01-01',
description: 'Published research on web development',
publicationUrl: 'https://journal.com/article',
},
];

// References
data.sections.references = [
{
id: 'ref-1',
employeeName: 'Jane Smith',
designation: 'Engineering Manager',
employer: 'Tech Corp',
contactNumber: '+1 234 567 8900',
},
];

// Social Media
data.sections.socialMedia = [
{
id: 'sm-1',
socialMediaName: 'Twitter',
socialLink: 'https://twitter.com/johndoe',
},
];

// Strengths
data.sections.strengths = [
{
id: 'str-1',
strengthName: 'Problem Solving',
strengthDescription: 'Excellent analytical skills',
},
];

// Volunteering
data.sections.volunteering = [
{
id: 'vol-1',
role: 'Mentor',
institutionName: 'Coding Bootcamp',
city: 'San Francisco',
country: 'USA',
startDate: '2022-01-01',
endDate: '2023-01-01',
isCurrent: false,
description: 'Mentored junior developers',
},
];

// Internships
data.sections.internships = [
{
id: 'int-1',
company: 'Startup Inc',
position: 'Software Intern',
city: 'San Francisco',
country: 'USA',
startDate: '2019-06-01',
endDate: '2019-12-01',
isCurrent: false,
description: 'Worked on frontend development projects',
},
];

// Summaries
data.sections.summaries = [
{
id: 'sum-1',
title: 'Professional Summary',
content: 'Experienced developer with 5+ years...',
},
];

// Custom Sections
data.sections.customSections = [
{
id: 'cus-1',
sectionTitle: 'Additional Information',
fields: [
{
label: 'Hobbies',
value: 'Reading, Hiking',
},
],
content: 'Additional details about my professional background',
},
];
Theme System
CSS Variables
Use CSS variables for theming support:

:root {
--primary-color: #3498db;
--secondary-color: #2c3e50;
--accent-color: #e74c3c;
--background-color: #ffffff;
--text-color: #333333;
--light-text: #7f8c8d;
--border-color: #bdc3c7;
}
Theme Application Function
function applyTheme(themeData) {
const root = document.documentElement;
const colors = themeData.colors;
Object.keys(colors).forEach((property) => {
root.style.setProperty(property, colors[property]);
});
}
Responsive Design
Mobile-First Approach
/_ Base styles for mobile _/
.container {
padding: 20px;
gap: 20px;
}

.resume-header {
flex-direction: column;
gap: 15px;
}

.profile-img-container {
width: 150px;
height: 150px;
}

.name {
font-size: 24px;
}

/_ Tablet styles _/
@media (min-width: 768px) {
.container {
padding: 30px;
gap: 30px;
}

.resume-header {
flex-direction: row;
gap: 20px;
}

.profile-img-container {
width: 180px;
height: 180px;
}
}

/_ Desktop styles _/
@media (min-width: 1024px) {
.container {
padding: 40px;
gap: 40px;
max-width: 1200px;
}

.profile-img-container {
width: 200px;
height: 200px;
}

.name {
font-size: 28px;
}
}
Testing Guidelines

1. Data Testing
   Test with various data scenarios:

// Test with full data
const fullData = {
personalInfo: {
/_ all fields _/
},
sections: {
/_ all sections with data _/
},
};

// Test with minimal data
const minimalData = {
personalInfo: {
firstName: 'John',
lastName: 'Doe',
},
sections: {},
};

// Test with missing fields
const incompleteData = {
personalInfo: {
fullName: 'John Doe',
// Missing other fields
},
sections: {
workExperience: [
{
position: 'Developer',
// Missing company, dates, etc.
},
],
},
}; 2. Theme Testing
Test with different theme configurations:

// Test predefined themes
const themes = [
{
theme: 'professional-blue',
colors: {
'--primary-color': '#3498db',
'--secondary-color': '#2c3e50',
'--background-color': '#ffffff',
},
},
{
theme: 'modern-green',
colors: {
'--primary-color': '#2ecc71',
'--secondary-color': '#27ae60',
'--background-color': '#f8f9fa',
},
},
];

themes.forEach((theme) => {
applyTheme(theme);
}); 3. Responsive Testing
Test on different screen sizes:

Mobile: 320px - 767px
Tablet: 768px - 1023px
Desktop: 1024px+ 4. Print Testing
Verify print layout:

Test print preview
Check page breaks
Ensure proper font sizing
Verify color contrast
Common Issues and Solutions

1. Element Not Found
   Always check if elements exist before manipulating them:

const element = document.getElementById('element-id');
if (element) {
// Safe to manipulate element
element.textContent = 'New content';
} 2. Data Validation
Validate data before rendering:

function renderSection(sectionData) {
if (!Array.isArray(sectionData) || sectionData.length === 0) {
return; // Exit early if no data
}

// Continue with rendering
} 3. Image Handling
Handle profile images properly:

function handleProfileImage(imageData) {
const imgElement = document.getElementById('profile-img');

if (imageData && imageData.trim()) {
imgElement.src = imageData.startsWith('data:')
? imageData
: `data:image/jpeg;base64,${imageData}`;
imgElement.style.display = 'block';
imgElement.onerror = () => {
imgElement.style.display = 'none';
};
} else {
imgElement.style.display = 'none';
}
} 4. Date Formatting
Handle various date formats:

function formatDate(dateString) {
if (!dateString) return '';

try {
const date = new Date(dateString);
return date.toLocaleDateString('en-US', {
year: 'numeric',
month: 'short',
});
} catch (error) {
return dateString; // Return original if parsing fails
}
}
Best Practices

1. Performance
   Minimize DOM manipulations
   Use document fragments for bulk updates
   Cache frequently accessed elements
   Debounce expensive operations
2. Accessibility
   Use semantic HTML elements
   Provide alt text for images
   Ensure proper color contrast
   Use ARIA attributes where needed
3. Maintainability
   Use consistent naming conventions
   Comment complex logic
   Modularize code into functions
   Follow DRY principles
4. Security
   Sanitize user input
   Validate data before rendering
   Use secure image handling
   Prevent XSS attacks
   Template Submission Checklist
   Before submitting a new template, ensure:

[ ] All required HTML elements are present with correct IDs
[ ] PostMessage event listener is implemented
[ ] All section rendering functions are included
[ ] Drag and drop functionality is working
[ ] Theme system is properly implemented
[ ] Responsive design is working on all devices
[ ] Print styles are optimized
[ ] All data fields are supported
[ ] Error handling is in place
[ ] Code is clean and well-commented
[ ] Template is tested with various data scenarios
[ ] Template works with all predefined themes
[ ] Template passes accessibility checks
This guide provides everything needed to create professional, feature-complete resume templates that integrate seamlessly with the existing system.
