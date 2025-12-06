<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Bloom Boutique</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #fafafa;
      color: #333;
    }
    header {
      background: #ff7eb9;
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 2rem;
      font-weight: bold;
    }
    .hero {
      background: url('https://images.unsplash.com/photo-1501004318641-b39e6451bec6') center/cover;
      height: 60vh;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 3rem;
      font-weight: bold;
      text-shadow: 0 0 10px rgba(0,0,0,0.6);
    }
    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      padding: 40px;
    }
    .card {
      background: white;
      border-radius: 15px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      overflow: hidden;
      text-align: center;
      padding-bottom: 10px;
    }
    .card img {
      width: 100%;
      height: 250px;
      object-fit: cover;
    }
    .card h3 {
      margin: 10px 0;
    }
    .card p {
      font-size: 1.1rem;
      margin-bottom: 10px;
    }
    .btn {
      background: #ff7eb9;
      padding: 10px 20px;
      border-radius: 10px;
      color: white;
      text-decoration: none;
      display: inline-block;
      margin-bottom: 15px;
      transition: 0.2s;
      cursor: pointer;
      border: none;
    }
    .btn:hover {
      background: #ff5fa3;
    }
    footer {
      background: #333;
      color: white;
      text-align: center;
      padding: 20px;
      margin-top: 40px;
    }

    /* Modal styles */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.5);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 1000;
    }
    .modal {
      background: white;
      padding: 20px;
      border-radius: 12px;
      width: 90%;
      max-width: 420px;
      box-shadow: 0 8px 30px rgba(0,0,0,0.2);
    }
    .modal h2 { margin-top: 0; }
    .field { margin-bottom: 10px; }
    .field label { display:block; font-size:0.9rem; margin-bottom:4px; }
    .field input, .field textarea {
      width: 100%;
      padding: 8px 10px;
      border-radius: 6px;
      border: 1px solid #ddd;
      font-size: 1rem;
      box-sizing: border-box;
    }
    .modal-actions { text-align: right; margin-top: 10px; }
    .secondary { background: #eee; color: #333; margin-right: 8px; }
  </style>
</head>
<body>
  <header>Bloom Boutique</header>

  <div class="hero">Fresh Flowers Delivered</div>

  <section class="products">
    <div class="card" data-name="Rose Bouquet" data-price="$29.99">
      <img src="https://images.unsplash.com/photo-1504208434309-cb69f4fe52b0" alt="Roses" />
      <h3>Rose Bouquet</h3>
      <p>$29.99</p>
      <button class="btn buy-btn">Buy Now</button>
    </div>

    <div class="card" data-name="Sunflower Bundle" data-price="$19.99">
      <img src="https://images.unsplash.com/photo-1524592094714-0f0654e20314" alt="Sunflowers" />
      <h3>Sunflower Bundle</h3>
      <p>$19.99</p>
      <button class="btn buy-btn">Buy Now</button>
    </div>

    <div class="card" data-name="Tulip Arrangement" data-price="$24.99">
      <img src="https://images.unsplash.com/photo-1526045612212-70caf35c14df" alt="Tulips" />
      <h3>Tulip Arrangement</h3>
      <p>$24.99</p>
      <button class="btn buy-btn">Buy Now</button>
    </div>
  </section>

  <footer>
    © 2025 Bloom Boutique • All Rights Reserved
  </footer>

  <!-- Modal -->
  <div class="modal-backdrop" id="modalBackdrop">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <h2 id="modalTitle">Send Order Email</h2>
      <p id="productInfo"></p>

      <div class="field">
        <label for="customerName">Your name</label>
        <input id="customerName" type="text" placeholder="Jane Doe" />
      </div>

      <div class="field">
        <label for="customerEmail">Your email</label>
        <input id="customerEmail" type="email" placeholder="you@example.com" />
      </div>

      <div class="field">
        <label for="message">Message (optional)</label>
        <textarea id="message" rows="4" placeholder="Add a note for the recipient or delivery instructions..."></textarea>
      </div>

      <div class="modal-actions">
        <button class="btn secondary" id="cancelBtn">Cancel</button>
        <button class="btn" id="sendBtn">Open Email App</button>
      </div>

      <small style="display:block;margin-top:8px;color:#666">This will open your default email application to compose a message to <strong>alimoesky@gmail.com</strong>. The email won't be sent until you confirm in your email app.</small>
    </div>
  </div>

  <script>
    // Updated to use Formspree for real email sending

    const modalBackdrop = document.getElementById('modalBackdrop');
    const productInfo = document.getElementById('productInfo');
    const customerName = document.getElementById('customerName');
    const customerEmail = document.getElementById('customerEmail');
    const message = document.getElementById('message');
    const sendBtn = document.getElementById('sendBtn');
    const cancelBtn = document.getElementById('cancelBtn');

    let currentProduct = null;

    document.querySelectorAll('.buy-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const card = e.target.closest('.card');
        const name = card.getAttribute('data-name');
        const price = card.getAttribute('data-price');
        currentProduct = { name, price };
        productInfo.textContent = `${name} — ${price}`;

        customerName.value = '';
        customerEmail.value = '';
        message.value = '';
        modalBackdrop.style.display = 'flex';
      });
    });

    cancelBtn.addEventListener('click', () => {
      modalBackdrop.style.display = 'none';
    });

    sendBtn.addEventListener('click', async () => {
      const orderData = {
        product: currentProduct.name,
        price: currentProduct.price,
        customerName: customerName.value,
        customerEmail: customerEmail.value,
        note: message.value,
      };

      // Formspree endpoint — replace with your own endpoint later
      const endpoint = "https://formspree.io/f/mvgzzvbd";

      try {
        await fetch(endpoint, {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(orderData)
        });

        alert("Order sent successfully! We’ll contact you shortly.");
      } catch (err) {
        alert("There was a problem sending your order. Please try again.");
      }

      modalBackdrop.style.display = 'none';
    });

    modalBackdrop.addEventListener('click', (e) => {
      if (e.target === modalBackdrop) modalBackdrop.style.display = 'none';
    });

    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') modalBackdrop.style.display = 'none';
    });
  </script>
</body>
</html>
