---
layout: default
title: EVE-NG Images | SP–DC–NET
permalink: /eve-ng-images/
---

<div class="page-heading">
  <span class="page-heading-icon">▣</span>
  <div>
    <h1>EVE-NG Images</h1>
    <p>Resource collections for your EVE-NG lab environment.</p>
  </div>
</div>

<p class="page-intro">
  Open a collection below. Your existing OneDrive destinations are preserved.
</p>

<div class="resource-layout">

  <!-- Main Content -->
  <main class="resource-main-column">

    <!-- Search -->
    <label class="search-box">
      <span>⌕</span>
      <input
        class="resource-search"
        type="search"
        placeholder="Search image collections..."
        aria-label="Search image collections"
      >
    </label>

    <!-- Image Collections -->
    <div class="image-grid">

      <!-- Cisco IOL -->
      <a
        class="image-card resource-card"
        data-category="Cisco IOL"
        data-search="cisco iol ios images"
        href="https://1drv.ms/f/c/f796b5a2ef0ab1d8/IgBy3oWotuHQT7cnJ4WC8n70AcFcifHGLwh7y5wd8ziE7M0?e=ZS8NeL"
      >
        <span class="image-card-icon">⌘</span>
        <strong>Cisco IOL Images</strong>
        <small>IOS / IOL lab images</small>
        <span class="download-label">↗ Open collection</span>
      </a>

      <!-- Dynamips -->
      <a
        class="image-card resource-card"
        data-category="Dynamips"
        data-search="dynamips router images"
        href="https://1drv.ms/f/c/f796b5a2ef0ab1d8/IgCXkOkjMCabQql4LkhZgdHsARso-cQXSKn5aielZPGPF0g?e=Ue52TL"
      >
        <span class="image-card-icon">◉</span>
        <strong>Dynamips Images</strong>
        <small>Dynamips-compatible images</small>
        <span class="download-label">↗ Open collection</span>
      </a>

      <!-- QEMU -->
      <a
        class="image-card resource-card"
        data-category="QEMU"
        data-search="qemu virtual machine images"
        href="https://1drv.ms/f/c/f796b5a2ef0ab1d8/IgDKZ1dxKjFiR5LS2uaF2DUIAXSgTawspOCCjig6VD5_BRk?e=IHMqYQ"
      >
        <span class="image-card-icon">☁</span>
        <strong>QEMU Images</strong>
        <small>QEMU virtual machine images</small>
        <span class="download-label">↗ Open collection</span>
      </a>

      <!-- Business -->
      <a
        class="image-card resource-card"
        data-category="Business"
        data-search="business images resources"
        href="https://1drv.ms/f/c/f796b5a2ef0ab1d8/IgCOQC4JRhHEQJlSputJXtCwAdz8eCPAtcU2exLSEWijqMk?e=715HaK"
      >
        <span class="image-card-icon">▤</span>
        <strong>Business</strong>
        <small>Additional resource collection</small>
        <span class="download-label">↗ Open collection</span>
      </a>

    </div>

    <!-- Empty Search Results -->
    <p class="no-results" hidden>
      No image collections match your search.
    </p>

  </main>

  <!-- Sidebar -->
  <aside class="resource-sidebar">

    <!-- Categories -->
    <section class="glass-panel">
      <h2>Image Categories</h2>

      <a class="sidebar-filter" href="#" data-filter="All">
        All Collections <span>4</span>
      </a>

      <a class="sidebar-filter" href="#" data-filter="Cisco IOL">
        Cisco IOL <span>1</span>
      </a>

      <a class="sidebar-filter" href="#" data-filter="Dynamips">
        Dynamips <span>1</span>
      </a>

      <a class="sidebar-filter" href="#" data-filter="QEMU">
        QEMU <span>1</span>
      </a>

      <a class="sidebar-filter" href="#" data-filter="Business">
        Business <span>1</span>
      </a>
    </section>

    <!-- Quick Links -->
    <section class="glass-panel">
      <h2>Quick Links</h2>

      <a
        class="sidebar-filter"
        href="{{ '/lab-writeups/' | relative_url }}"
      >
        ⚗ Lab Writeups
      </a>

      <a
        class="sidebar-filter"
        href="{{ '/networking-notes/' | relative_url }}"
      >
        ▤ Networking Notes
      </a>
    </section>

  </aside>

</div>

<!-- Search and Category Filtering -->
<script>
document.addEventListener('DOMContentLoaded', function () {
  const input = document.querySelector('.resource-search');
  const cards = [
    ...document.querySelectorAll('.resource-card')
  ];

  const filters = [
    ...document.querySelectorAll('[data-filter]')
  ];

  const empty = document.querySelector('.no-results');

  let activeCategory = 'All';

  function updateCollections() {
    const query = input
      ? input.value.trim().toLowerCase()
      : '';

    let visibleCount = 0;

    cards.forEach(function (card) {
      const categoryMatches =
        activeCategory === 'All' ||
        card.dataset.category === activeCategory;

      const searchMatches =
        card.dataset.search.includes(query);

      const visible =
        categoryMatches && searchMatches;

      card.hidden = !visible;

      if (visible) {
        visibleCount++;
      }
    });

    if (empty) {
      empty.hidden = visibleCount !== 0;
    }
  }

  filters.forEach(function (filter) {
    filter.addEventListener('click', function (event) {
      if (!filter.dataset.filter) {
        return;
      }

      event.preventDefault();

      activeCategory = filter.dataset.filter;
      updateCollections();
    });
  });

  if (input) {
    input.addEventListener('input', updateCollections);
  }

  updateCollections();
});
</script>
