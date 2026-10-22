# Azenly

**Bring Back Memory**

Azenly is a student project focused on sharing information about Alzheimer's disease and creating a space for community content. The project introduces an initiative by students from Le Quy Don High School for the Gifted in Vung Tau, Vietnam.

This repository contains two parts: a static landing page and a Django application with a multi-author blog and an author dashboard. The repository is named `alzenly`, while the website uses the name **Azenly**.

## Project Overview

- **Landing page:** introduces the project, presents educational content, links to an external article, and includes information and contact sections.
- **Django application:** provides account registration, authentication, blog content, and content management screens.

The project is a prototype under development. Some pages and workflows are incomplete; see [Known Limitations](#known-limitations) before running it.

## Features

### Public Website

- Static landing page built with the LeadMark template.
- Sections introducing Alzheimer's disease and the project team.
- Blog detail pages with view counts and related posts by the same author.
- Category pages with pagination.
- Search by post title, category name, and content.
- Comment submission.
- Email subscription storage.

### Accounts and Content Management

- User registration, login, and logout.
- Author profiles and profile editing.
- Dashboard statistics for an author's posts and views.
- Post creation, editing, deletion, and visibility controls.
- Post status fields for `pending` and `active` content.
- Category management and Django's built-in administration interface.

These features are represented in the source code; they should not be interpreted as a claim that every workflow has been tested or completed.

## Technology Stack

| Component | Technology |
| --- | --- |
| Backend | Python, Django 3.1.5 |
| Database | SQLite |
| Frontend | HTML, CSS, JavaScript, Bootstrap, jQuery |
| Templates | LeadMark, Mundana, SB Admin 2 |
| Image handling | Pillow 8.1.2 |
| REST framework dependency | Django REST Framework 3.12.4 |

Django REST Framework is installed, but the repository does not currently provide a completed application API.

## Repository Structure

```text
alzenly/
├── index.html              # Static Azenly landing page
├── components.html        # Additional static HTML page
├── assets/                # Landing page styles, scripts, images, and vendors
└── backend/
    ├── manage.py          # Django management entry point
    ├── requirments.txt     # Python dependencies (original filename)
    ├── src/               # Django settings and root URL configuration
    ├── blog/              # Blog models, views, and routes
    ├── dashboard/         # Author accounts and content management
    ├── api/               # API scaffold
    ├── templates/         # Django HTML templates
    ├── static/            # Backend frontend assets
    ├── images/            # Media files and reference screenshots
    └── docs/              # Existing project documentation
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/nvkhoa1207/alzenly.git
cd alzenly
```

### 2. Preview the Static Landing Page

From the repository root, start a local static server:

```bash
python3 -m http.server 5500 --bind 127.0.0.1
```

On Windows, use `python` instead of `python3` if needed. Open [http://127.0.0.1:5500](http://127.0.0.1:5500).

This serves the landing page only. Its login button currently links to `http://127.0.0.1:8000/`, so the Django server must also be running to follow that link locally.

### 3. Set Up the Django Application

Open another terminal and enter the `backend` directory:

```bash
cd alzenly/backend
```

If your terminal is already at the repository root, use `cd backend` instead.

Create and activate a virtual environment:

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows PowerShell**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
python -m pip install -r requirments.txt
```

> The dependency file is named `requirments.txt`, not `requirements.txt`. It pins older packages, including Django 3.1.5 and Pillow 8.1.2. Installation on a recent Python version may require dependency updates or a compatible legacy environment. The repository does not specify a tested Python version.

Initialize the database, create an administrator, and start the development server:

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

### 4. Initialize Content

1. Open [Django Admin](http://127.0.0.1:8000/admin/) and sign in with the superuser account.
2. Create an `Author` record linked to that account before using the author dashboard. Creating a superuser does not automatically create an author profile.
3. Create a category with a valid slug.
4. Create at least two blog posts linked to an author and category, with `status` set to `active`, `visible` enabled, and `featured` enabled.

The two featured posts are necessary because the current homepage view directly accesses the second featured post. Without enough data, the homepage can raise an `IndexError`. The homepage template itself also remains incomplete.

## Local Routes

| Route | Purpose |
| --- | --- |
| `http://127.0.0.1:5500/` | Static landing page |
| `http://127.0.0.1:8000/` | Django homepage |
| `http://127.0.0.1:8000/sign-up/` | Account registration |
| `http://127.0.0.1:8000/login/` | Login |
| `http://127.0.0.1:8000/dashboard/` | Author dashboard |
| `http://127.0.0.1:8000/admin/` | Django administration |
| `http://127.0.0.1:8000/blog/<id>/` | Blog post detail |
| `http://127.0.0.1:8000/topic/<slug>` | Category page |
| `http://127.0.0.1:8000/search/?q=<keyword>` | Blog search |

Replace `<id>`, `<slug>`, and `<keyword>` with actual values.

## Known Limitations

- The static landing page and Django application are served separately. The landing page's login link points to a local address and must be updated for a hosted deployment.
- The Django homepage assumes at least two active, visible, featured posts exist. Its body template is largely empty.
- The landing page's contact form has no configured submission endpoint.
- Email subscriptions are stored in the database; an automated newsletter delivery service is not implemented.
- Tag functionality is incomplete: the public tag view references a missing template, and some dashboard tag routes do not match their view parameters.
- The pending-post listing filters on `Pending`, while the model uses `pending`.
- Editing a post assigns its category to `cat_obj` rather than the model's `catagories` field, so category changes need correction.
- Several post operations check authentication without checking ownership. Access control needs review before multi-user deployment.
- Development settings include `DEBUG = True`, an embedded secret key, and an empty `ALLOWED_HOSTS` list. Production configuration and dependency updates are still needed.

The setup instructions above are based on source inspection; they have not been validated through a full installation and end-to-end run.

## Contributing

Bug reports and improvements can be submitted through [GitHub Issues](https://github.com/nvkhoa1207/alzenly/issues) or a pull request.

Useful areas for contribution include completing the homepage, fixing content-management workflows, improving access controls, updating dependencies, and adding tests for key user flows.

## Acknowledgments

- The backend includes code and documentation from [Multi-Authors Advanced Django Blog](https://github.com/yeazin/Multi-Authors-advanced-Django-Blog) by Nazrul Islam Yeasin.
- The static landing page uses the LeadMark template by DevCrud.
- The backend documentation credits the Mundana and SB Admin 2 templates.

## License

This repository does not currently contain a complete project license. The existing backend README mentions MIT licensing for its upstream project, but `backend/LICENSE` does not contain license terms. Confirm the applicable permissions and retain upstream attribution when reusing or distributing the code and bundled assets.
