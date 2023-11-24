<script lang="ts">
    import '$lib/css/chota.min.css';
    import '$lib/css/style.css';
    // import SvelteSeo from "svelte-seo";
    import { base } from '$app/paths';
    </script>
    
    <!-- <SvelteSeo
      title="Alih Aksara"
      description="Alat transliterasi sederhana untuk mengalihkan/mengubah/mengkonversi teks aksara Jawa ke bentuk aksara lain ataupun sebaliknya."
      canonical="https://adityarahmanda.github.io/alih-aksara"
      keywords="alat, transliterasi, konversi, alih, aksara, jawa, latin, kawi"
      openGraph={{
        title: "Alih Aksara",
        description: "Alat transliterasi sederhana untuk mengalihkan/mengubah/mengkonversi teks aksara Jawa ke bentuk aksara lain ataupun sebaliknya.",
        url: "https://adityarahmanda.github.io/alih-aksara",
        type: "website",
        images: [{
            url: "https://adityarahmanda.github.io/alih-aksara/alih-aksara-screenshot.png",
          },],
        site_name: "Alih Aksara",
      }}
      twitter={{
        card: "summary_large_image",
        title: "Alih Aksara",
        description: "Alat transliterasi sederhana untuk mengalihkan/mengubah/mengkonversi teks aksara Jawa ke bentuk aksara lain ataupun sebaliknya.",
        image: "https://adityarahmanda.github.io/alih-aksara/alih-aksara-screenshot.png",
      }}
    /> -->
    
    <section>
      
      <div class="row hero is-vertical-align">
        <div class="col illustration-image" style="text-align: left;">
          <picture>
            <source type="image/webp" media="(min-width:900px)" width="420" srcset="{ base }/La_galigo.jpg">
            <source type="image/webp" width="360" srcset="{ base }/La_galigo.jpg">
            <img src="{ base }/La_galigo.jpg"  alt="Illustration">
          </picture>
        </div>
        <div class="col" style="margin-left: 5%;">
          <h1 style="font-weight: 900; font-size:8rem; margin-bottom:0; margin-top:-15%;">ᨒᨚᨈᨑ <span style="font-weight: 200;"> link</span></h1>
          <p>Seamlessly transliterate Latin script to Lontara with a single click, preserving linguistic authenticity and cultural richness. Break through language barriers and elevate your research connectivity. </p>
          <a href="{ base }/latin-lontara" class="button primary">Try Now</a>
        </div>
      </div>
    
      <!-- <div class="row">
        <div class="col-8">
          <h4>Sekilas Tentang alih aksara</h4>
          <p>Bisa dibilang proyek <strong>alih aksara</strong> ini hanyalah sebuah keisengan. Iseng karena ini memang bukan proyek serius apalagi resmi, sekadar proyek pribadi suka-suka yang saya manfaatkan untuk lebih dalam mempelajari aksara Jawa ataupun ilmu pemrograman web itu sendiri.</p> 
          <p>Oleh karena itu, saya yang masih awam ini dengan tangan terbuka menerima segala umpan balik untuk mengembangkan website ini menjadi lebih baik.</p>
        </div>
        <div class="col">
          <div style="margin-bottom: 2em">
            <h4>Umpan Balik</h4>
            <span>Kirimkan saja melalui <a href="mailto: arahmanda21@gmail.com" style="text-decoration: underline;">Email</a> ini</span>
          </div>
          <div>
            <h4>Mungkin kamu juga tertarik</h4>
            <ul class="list-unstyled">
              <li style="margin-bottom: .5em">
                <a href="https://adityarahmanda.github.io/macaksara/" style="text-decoration: underline;">Macaksara - Website Permainan Kuis Aksara Jawa</a>
              </li>
              <li style="margin-bottom: .5em">
                <a href="https://adityarahmanda.github.io/artikel/belajar-aksara-jawa" style="text-decoration: underline;">Belajar Aksara Jawa Lengkap Beserta Contohnya</a>
              </li>
            </ul>
          </div>
        </div>
      </div> -->
    
    </section>