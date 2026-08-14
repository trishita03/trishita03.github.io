// Mobile nav toggle
const toggle = document.querySelector('.nav-toggle');
const links = document.querySelector('.nav-links');
if (toggle && links) {
  toggle.addEventListener('click', () => {
    links.classList.toggle('open');
  });
  links.querySelectorAll('a').forEach((a) => {
    a.addEventListener('click', () => links.classList.remove('open'));
  });
}

// Scroll reveal
const revealEls = document.querySelectorAll('.reveal');
if ('IntersectionObserver' in window && revealEls.length) {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.12 }
  );
  revealEls.forEach((el) => observer.observe(el));
} else {
  revealEls.forEach((el) => el.classList.add('is-visible'));
}

// Scroll spy for nav active state
const sections = document.querySelectorAll('main section[id]');
const navLinks = document.querySelectorAll('.nav-link');
if (sections.length && navLinks.length && 'IntersectionObserver' in window) {
  const spy = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const id = entry.target.getAttribute('id');
          navLinks.forEach((link) => {
            link.classList.toggle('active', link.getAttribute('href') === `#${id}`);
          });
        }
      });
    },
    { rootMargin: '-45% 0px -50% 0px' }
  );
  sections.forEach((section) => spy.observe(section));
}

// Skill bubbles: drag to move, click to expand
const bubbleField = document.getElementById('bubbleField');
if (bubbleField) {
  const bubbles = bubbleField.querySelectorAll('.bubble');

  const collapseAll = (except) => {
    bubbles.forEach((b) => {
      if (b !== except) b.classList.remove('expanded');
    });
  };

  const toggleExpand = (el) => {
    const wasExpanded = el.classList.contains('expanded');
    collapseAll(null);
    if (!wasExpanded) el.classList.add('expanded');
  };

  bubbles.forEach((el) => {
    let dragging = false;
    let moved = false;
    let startX = 0, startY = 0, origLeft = 0, origTop = 0;

    const onPointerDown = (e) => {
      dragging = true;
      moved = false;
      el.setPointerCapture(e.pointerId);
      const rect = el.getBoundingClientRect();
      const fieldRect = bubbleField.getBoundingClientRect();
      origLeft = rect.left - fieldRect.left;
      origTop = rect.top - fieldRect.top;
      startX = e.clientX;
      startY = e.clientY;
      el.style.left = origLeft + 'px';
      el.style.top = origTop + 'px';
      el.classList.add('dragging');
    };

    const onPointerMove = (e) => {
      if (!dragging) return;
      const dx = e.clientX - startX;
      const dy = e.clientY - startY;
      if (Math.abs(dx) > 4 || Math.abs(dy) > 4) moved = true;
      const maxLeft = bubbleField.clientWidth - el.offsetWidth;
      const maxTop = bubbleField.clientHeight - el.offsetHeight;
      const newLeft = Math.max(0, Math.min(maxLeft, origLeft + dx));
      const newTop = Math.max(0, Math.min(maxTop, origTop + dy));
      el.style.left = newLeft + 'px';
      el.style.top = newTop + 'px';
    };

    const onPointerUp = (e) => {
      if (!dragging) return;
      dragging = false;
      el.classList.remove('dragging');
      try { el.releasePointerCapture(e.pointerId); } catch (err) {}
      if (!moved) toggleExpand(el);
    };

    el.addEventListener('pointerdown', onPointerDown);
    el.addEventListener('pointermove', onPointerMove);
    el.addEventListener('pointerup', onPointerUp);
    el.addEventListener('pointercancel', onPointerUp);

    // keyboard accessibility: Enter/Space toggles expand
    el.addEventListener('keydown', (e) => {
      if (e.key === 'Enter' || e.key === ' ') {
        e.preventDefault();
        toggleExpand(el);
      }
    });
  });

  // click outside collapses any expanded bubble
  document.addEventListener('pointerdown', (e) => {
    if (!bubbleField.contains(e.target)) collapseAll(null);
  });
}

// Contact form submission via Formspree (AJAX, no page redirect)
const contactForm = document.getElementById('contactForm');
const formStatus = document.getElementById('formStatus');
if (contactForm) {
  contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    formStatus.textContent = 'Sending…';
    formStatus.className = 'form-status';
    submitBtn.disabled = true;

    try {
      const response = await fetch(contactForm.action, {
        method: 'POST',
        body: new FormData(contactForm),
        headers: { 'Accept': 'application/json' },
      });
      if (response.ok) {
        formStatus.textContent = "Thanks — your message is on its way. I'll get back to you soon.";
        formStatus.className = 'form-status success';
        contactForm.reset();
      } else {
        formStatus.textContent = 'Something went wrong. Please try emailing me directly instead.';
        formStatus.className = 'form-status error';
      }
    } catch (err) {
      formStatus.textContent = 'Something went wrong. Please try emailing me directly instead.';
      formStatus.className = 'form-status error';
    } finally {
      submitBtn.disabled = false;
    }
  });
}
