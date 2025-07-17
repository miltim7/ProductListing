<!-- src\components\ProductsTable.vue -->

<template>
  <div class="container">
    <div class="tabs">
      <button class="tab active">
        Предложения
        <span class="tab-badge">12</span>
      </button>
      <button class="tab">
        Галереи
        <span class="tab-badge">78</span>
      </button>
      <button class="tab">
        Избранное
        <span class="tab-badge">179</span>
      </button>
      <button class="tab">
        Выставки
        <span class="tab-badge">2</span>
      </button>
      <button class="tab">
        Новости
        <span class="tab-badge">2</span>
      </button>
    </div>

    <div class="tabs-mobile">
      <select class="tabs-select">
        <option value="proposals" selected>Предложения 12</option>
        <option value="gallery">Галерея 15</option>
        <option value="favorites">Избранное 175</option>
        <option value="exhibitions">Выставки 3</option>
        <option value="news">Новости 2</option>
      </select>
    </div>

    <div class="filters">
      <div class="filters-left">
        <select class="filter-select">
          <option value="active">Активные (4)</option>
          <option value="pending">В ожидании (2)</option>
          <option value="draft">Черновики (1)</option>
        </select>
        <select class="filter-select">
          <option value="inactive">Не активные (8)</option>
          <option value="expired">Истекшие (3)</option>
          <option value="blocked">Заблокированные (1)</option>
        </select>
        <select class="filter-select">
          <option value="sales">Сортировка по папкам</option>
          <option value="date">По дате создания</option>
          <option value="price">По цене</option>
          <option value="name">По названию</option>
        </select>
      </div>
      <div class="filters-right">
        <label class="toggle-switch">
          <input type="checkbox" v-model="editMode" />
          <span class="slider"></span>
        </label>
        <span>Редактировать</span>
        <div class="view-buttons">
          <button class="view-btn">
            <img src="/images/burger-icon.png" alt="list" class="view-icon view-icon1" />
          </button>
          <button class="view-btn active">
            <img src="/images/grid-icon.png" alt="grid" class="view-icon view-icon2" />
          </button>
        </div>
      </div>
    </div>

    <div class="products-list">
      <div
        v-for="(product, index) in products"
        :key="product.id + '_' + index"
        class="product-item"
      >
        <img :src="product.image" :alt="product.title" class="product-image" />

        <div class="product-info">
          <div class="product-title">{{ product.title }}</div>
        </div>

        <div class="product-id">#{{ product.id }}</div>

        <div class="product-price">
          {{ product.price }}
        </div>

        <button class="promo-btn" @click="openPromoModal">
          <img src="/images/promo-icon.png" alt="promo" class="promo-icon" />
          Promo
        </button>

        <div class="product-status" 
             :class="{ 'product-status--error': product.status === 'error' }"
             @mouseenter="showStatusTooltipHandler(index)"
             @mouseleave="hideStatusTooltip"
             @click="toggleStatusTooltip(index)">
          <img 
            :src="product.status === 'error' ? '/images/refresh-icon-red.png' : '/images/refresh-icon.png'" 
            alt="refresh" 
            class="status-icon" 
          />
          <span class="status-text" :class="{ 'status-text--error': product.status === 'error' }">
            {{ product.phone }}
          </span>
          
          <div class="status-tooltip" :class="{ show: showStatusTooltip && statusTooltipIndex === index }">
            <div class="tooltip-content">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
            </div>
          </div>
        </div>

        <div class="product-actions">
          <button class="actions-btn" @click="toggleDropdown(index)">
            <span class="icon-dots"></span>
          </button>

          <div class="dropdown-menu" :class="{ show: dropdownIndex === index }">
            <button class="dropdown-item">
              <img src="/images/pen-icon.png" alt="edit" class="dropdown-icon-img" />
              Обновить
            </button>
            <button class="dropdown-item">
              <img src="/images/arrow-icon.png" alt="promote" class="dropdown-icon-img" />
              Продвигать
            </button>
            <button class="dropdown-item">
              <img src="/images/money-icon.png" alt="price" class="dropdown-icon-img" />
              Изменить цену
            </button>
            <button class="dropdown-item">
              <img src="/images/certificate-icon.png" alt="certificate" class="dropdown-icon-img" />
              Депозитный сертификат
            </button>
            <button class="dropdown-item">
              <img src="/images/trophy-icon.png" alt="trophy" class="dropdown-icon-img" />
              Участвуй в ...
            </button>
            <button class="dropdown-item">
              <img src="/images/trash-icon.png" alt="delete" class="dropdown-icon-img" />
              Удалить
            </button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="showPromoModal" class="modal-overlay" @click="closePromoModal">
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <h3 class="modal-title">Получить больше просмотров</h3>
          <button class="modal-close" @click="closePromoModal">
            <img src="/images/x.png" alt="закрыть" class="close-icon" />
          </button>
        </div>

        <div class="modal-body">
          <div class="promo-option">
            <div class="option-title">Показывать на главной странице в блоке Promo:</div>
            <div class="option-row">
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.mainPageNo" />
                <span class="checkbox-custom"></span>
                Нет
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.mainPageCoins" checked />
                <span class="checkbox-custom"></span>
                <span class="coins-badge">2 ArtCoins</span>
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.mainPagePoints" />
                <span class="checkbox-custom"></span>
                1200 баллов
              </label>
            </div>
          </div>

          <div class="promo-option">
            <div class="option-title">Показывать работы в Vip-блоке:</div>
            <div class="option-row">
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.vipBlockNo" />
                <span class="checkbox-custom"></span>
                Нет
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.vipBlockCoins" checked />
                <span class="checkbox-custom"></span>
                <span class="coins-badge">4 ArtCoins</span>
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.vipBlockPoints" />
                <span class="checkbox-custom"></span>
                2400 баллов
              </label>
            </div>
          </div>

          <div class="promo-option">
            <div class="option-title">Пост в социальных сетях:</div>
            <div class="option-row">
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.socialPostNo" />
                <span class="checkbox-custom"></span>
                Нет
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.socialPostCoins" checked />
                <span class="checkbox-custom"></span>
                <span class="coins-badge">8 ArtCoins</span>
              </label>
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.socialPostPoints" />
                <span class="checkbox-custom"></span>
                4800 баллов
              </label>
            </div>
          </div>

          <div class="promo-option">
            <div class="option-title">Показывать на главной странице в блоке Promo:</div>
            <div class="option-row single">
              <label class="checkbox-option">
                <input type="checkbox" v-model="promoData.freePromo" checked />
                <span class="checkbox-custom"></span>
                Бесплатно
              </label>
            </div>
          </div>
        </div>

        <div class="modal-footer">
          <button class="apply-btn" @click="applyPromo">Применить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductsTable",
  data() {
    return {
      editMode: false,
      dropdownIndex: -1,
      showPromoModal: false,
      showStatusTooltip: false,
      statusTooltipIndex: -1,
      promoData: {
        mainPageNo: false,
        mainPageCoins: true,
        mainPagePoints: false,
        vipBlockNo: false,
        vipBlockCoins: true,
        vipBlockPoints: false,
        socialPostNo: false,
        socialPostCoins: true,
        socialPostPoints: false,
        freePromo: true,
      },
      products: [
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "6 650,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1578662996442-48f60103fc96?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "60,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1586023492125-27b2c045efd7?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "700,000 ₽",
          phone: "15г.04ч.44м",
          status: "error",
          image:
            "https://images.unsplash.com/photo-1513475382585-d06e58bcb0e0?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "600,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1567538096630-e0c55bd6374c?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "61,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1558618666-fbd697c83667?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "8,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1484154218962-a197022b5858?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "70,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1493663284031-b7e3aaa4cab7?w=60&h=60&fit=crop&crop=center",
        },
        {
          id: "84634463",
          title: "Home decor Skull with roses for living room",
          price: "65,000 ₽",
          phone: "15г.04ч.44м",
          status: "active",
          image:
            "https://images.unsplash.com/photo-1540932239986-30128078f3c5?w=60&h=60&fit=crop&crop=center",
        },
      ],
    };
  },
  methods: {
    toggleDropdown(index) {
      this.dropdownIndex = this.dropdownIndex === index ? -1 : index;
    },
    openPromoModal() {
      this.showPromoModal = true;
      document.body.style.overflow = "hidden";
    },
    closePromoModal() {
      this.showPromoModal = false;
      document.body.style.overflow = "auto";
    },
    applyPromo() {
      console.log("Promo applied:", this.promoData);
      this.closePromoModal();
    },
    showStatusTooltipHandler(index) {
      this.statusTooltipIndex = index;
      this.showStatusTooltip = true;
    },
    hideStatusTooltip() {
      this.showStatusTooltip = false;
      this.statusTooltipIndex = -1;
    },
    toggleStatusTooltip(index) {
      if (this.statusTooltipIndex === index && this.showStatusTooltip) {
        this.hideStatusTooltip();
      } else {
        this.showStatusTooltipHandler(index);
      }
    }
  },
  mounted() {
    document.addEventListener("click", (e) => {
      if (!e.target.closest(".product-actions")) {
        this.dropdownIndex = -1;
      }
      if (!e.target.closest(".product-status")) {
        this.hideStatusTooltip();
      }
    });
  },
};
</script>