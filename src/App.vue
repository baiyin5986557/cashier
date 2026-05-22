<template>
  <!-- ====== 首页（选择角色） ====== -->
  <div class="home-page" v-if="page === 'home'">
    <div class="home-curve"></div>
    <div class="home-brand">
      <img class="home-logo" src="/logo.png" alt="花花世界" />
      <div class="home-title">花花世界</div>
    </div>
    <div class="home-subtitle">花漾花店</div>
    <div class="home-btns">
      <button class="home-btn home-btn-user" @click="goUserPay">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
          <rect x="2" y="4" width="20" height="16" rx="3" stroke="#fff" stroke-width="1.8"/>
          <path d="M7 10h10M7 14h6" stroke="#fff" stroke-width="1.8" stroke-linecap="round"/>
        </svg>
        <span>用户</span>
      </button>
      <button class="home-btn home-btn-merchant" @click="goMerchantCollect">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
          <path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" stroke="#1B5E3C" stroke-width="1.8" stroke-linejoin="round"/>
          <path d="M9 21V12h6v9" stroke="#1B5E3C" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span>商户</span>
      </button>
      <button class="home-btn home-btn-agent" @click="goAgentOrder">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
          <path d="M4 4h16v16H4V4z" stroke="#B8860B" stroke-width="1.8" stroke-linejoin="round"/>
          <path d="M8 8h8v8H8V8z" stroke="#B8860B" stroke-width="1.8" stroke-linejoin="round"/>
          <path d="M12 8v8M8 12h8" stroke="#B8860B" stroke-width="1.8" stroke-linecap="round"/>
        </svg>
        <span>代客</span>
      </button>
      <button class="home-btn home-btn-record" @click="goRecord">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
          <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8l-6-6z" stroke="#1565C0" stroke-width="1.8" stroke-linejoin="round"/>
          <path d="M14 2v6h6M16 13H8M16 17H8M10 9H8" stroke="#1565C0" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        <span>录单</span>
      </button>
    </div>
    <!-- 底部装饰文字 -->
    <div class="home-footer-text">— 花间有礼 心意直达 —</div>
  </div>

  <!-- ====== 用户扫码付款页（银联风格） ====== -->
  <div class="union-page" v-if="page === 'userPay'">
    <div class="union-top-card">
      <div class="union-merchant">
        <img src="/logo.png" class="union-merchant-logo" />
        <span class="union-merchant-name">花漾花店</span>
      </div>
    </div>

    <!-- 订单金额 -->
    <div class="union-amount-block">
      <div class="union-amount-label">订单金额</div>
      <div class="union-amount-display">
        <span class="union-currency">¥</span>
        <span class="union-amount-text">{{ currentAmountStr || '0.00' }}</span>
      </div>
    </div>

    <!-- 商品图片（折叠） -->
    <div class="union-images-toggle" @click="showUserImages = !showUserImages">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <rect x="2" y="2" width="20" height="20" rx="3" stroke="#2E7D52" stroke-width="1.5"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
        <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
        <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <span>拍照上传商品图片 ({{ imageList.length }}/10)</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" :class="{'arrow-up': showUserImages}">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="union-images" v-if="showUserImages">
      <div class="union-img-item" v-for="(img, idx) in imageList" :key="idx">
        <img :src="img" />
        <div class="union-img-remove" @click="removeImage(idx)">×</div>
      </div>
      <label class="union-img-add" v-if="imageList.length < 10">
        <input type="file" accept="image/*" capture="environment" @change="onFileChange($event, 'user')" style="display:none" />
        <svg width="24" height="24" viewBox="0 0 32 32" fill="none">
          <path d="M16 6v20M6 16h20" stroke="#C8C9CC" stroke-width="2.5" stroke-linecap="round"/>
        </svg>
      </label>
    </div>
    <div class="union-remark-inline" @click="showRemarkDialog=true">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <path d="M16 2v4a2 2 0 002 2h4" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M18 22H6a2 2 0 01-2-2V4a2 2 0 012-2h7l7 7v11a2 2 0 01-2 2z" stroke="#2E7D52" stroke-width="1.5" stroke-linejoin="round"/>
        <path d="M12 11v6" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round"/>
        <path d="M9 14h6" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <span class="union-remark-inline-text" v-if="remark">{{ remark }}</span>
      <span class="union-remark-inline-placeholder" v-else>备注附言</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" :class="{'arrow-up': false}">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="deduction-divider"></div>
    <!-- 抵扣方式 -->
    <div class="union-deductions" style="display:none">
      <div class="deduction-row">
        <span class="deduction-label-row">
          <span class="deduction-label">福利金</span>
          <span class="deduction-balance">余额：{{ welfareBalance }}</span>
        </span>
        <label class="deduction-toggle">
          <input type="checkbox" :checked="userDeductions.welfare > 0" @change="userDeductions.welfare = $event.target.checked ? welfareBalance : 0" />
          <span class="toggle-track"><span class="toggle-knob"></span></span>
        </label>
      </div>
      <div class="deduction-row">
        <span class="deduction-label">礼品卡</span>
        <span class="deduction-giftcard" v-if="userDeductions.giftCard > 0" @click="openDeductionInput('user', 'giftCard')">{{ userDeductions.giftCard }}元</span>
        <span class="deduction-add" v-else @click="openDeductionInput('user', 'giftCard')">选择</span>
      </div>
    </div>
    <div class="union-numpad">
      <div class="union-numpad-keys">
        <button class="union-key" v-for="n in 9" :key="n" @click="numpadInput(String(n))">{{ n }}</button>
        <button class="union-key union-key-zero" colspan="2" @click="numpadInput('0')">0</button>
        <button class="union-key union-key-dot" @click="numpadDot">.</button>
      </div>
      <div class="union-numpad-actions">
        <button class="union-del-btn" @click="numpadDel">×</button>
        <button class="union-confirm-btn" @click="handlePay" type="button">
          付 款
        </button>
      </div>
    </div>
  </div>

  <!-- ====== 代客下单 - 商品分类列表页 ====== -->
  <div class="agent-page" v-if="page === 'agentShop'">
    <div class="agent-header">
      <div class="agent-top">
        <div class="agent-top-left" @click="page='home'">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M15 18l-6-6 6-6" stroke="#fff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span>返回</span>
        </div>
        <span class="agent-top-title">花漾花店</span>
        <div class="agent-top-cart" @click="goAgentCart">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <circle cx="9" cy="21" r="1.5" fill="#fff"/>
            <circle cx="20" cy="21" r="1.5" fill="#fff"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="agent-cart-badge" v-if="cartTotal > 0">{{ cartTotal }}</span>
        </div>
      </div>
    </div>

    <!-- 分类标签栏 -->
    <div class="agent-tabs">
      <div class="agent-tab" v-for="(cat, ci) in categories" :key="ci" :class="{active: activeCategory === ci}" @click="activeCategory = ci">
        {{ cat.name }}
      </div>
    </div>

    <!-- 商品列表 -->
    <div class="agent-products">
      <div class="agent-product" v-for="(prod, pi) in currentProducts" :key="pi" @click="addToCart(prod)">
        <div class="agent-prod-img">
          <div class="agent-prod-placeholder">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none">
              <rect x="2" y="2" width="20" height="20" rx="3" stroke="#ccc" stroke-width="1.2"/>
              <path d="M21 15l-5-5-9 9" stroke="#ccc" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
        <div class="agent-prod-info">
          <div class="agent-prod-name">{{ prod.name }}</div>
          <div class="agent-prod-spec" v-if="prod.spec">{{ prod.spec }}</div>
          <div class="agent-prod-price">
            <span class="agent-prod-price-symbol">¥</span>
            <span class="agent-prod-price-val">{{ prod.price }}</span>
          </div>
        </div>
        <div class="agent-prod-add">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <circle cx="12" cy="12" r="10" fill="#DAA520"/>
            <path d="M12 7v10M7 12h10" stroke="#fff" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
      </div>
    </div>

    <!-- 底部购物车栏 -->
    <div class="agent-bottom-bar" v-if="cartTotal > 0" @click="goAgentCart">
      <div class="agent-bottom-left">
        <div class="agent-bottom-cart-icon">
          <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
            <circle cx="9" cy="21" r="2" fill="#DAA520"/>
            <circle cx="20" cy="21" r="2" fill="#DAA520"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#DAA520" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="agent-bottom-badge">{{ cartTotal }}</span>
        </div>
        <div class="agent-bottom-total">
          <span class="agent-bottom-label">合计</span>
          <span class="agent-bottom-amount">¥{{ cartTotalPrice }}</span>
        </div>
      </div>
      <div class="agent-bottom-pay" @click.stop="goAgentCart">去结算</div>
    </div>
  </div>

  <!-- ====== 代客下单 - 购物车页 ====== -->
  <div class="agent-page" v-if="page === 'agentCart'">
    <div class="agent-header">
      <div class="agent-top">
        <div class="agent-top-left" @click="page='agentShop'">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M15 18l-6-6 6-6" stroke="#fff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span>返回</span>
        </div>
        <span class="agent-top-title">购物车</span>
        <div class="agent-top-clear" @click="clearCart" v-if="cart.length > 0">清空</div>
      </div>
    </div>

    <div class="agent-cart-list" v-if="cart.length > 0">
      <div class="agent-cart-item" v-for="(item, ci) in cart" :key="ci">
        <div class="agent-cart-item-left">
          <div class="agent-cart-info">
            <div class="agent-cart-name">{{ item.name }}</div>
            <div class="agent-cart-spec" v-if="item.spec">{{ item.spec }}</div>
          </div>
        </div>
        <div class="agent-cart-item-right">
          <div class="agent-cart-price">¥{{ item.price }}</div>
          <div class="agent-cart-qty">
            <button class="agent-qty-btn" @click="cartQty(ci, -1)">−</button>
            <span class="agent-qty-val">{{ item.qty }}</span>
            <button class="agent-qty-btn" @click="cartQty(ci, 1)">+</button>
          </div>
        </div>
      </div>
    </div>
    <div class="agent-cart-empty" v-else>
      <svg width="48" height="48" viewBox="0 0 24 24" fill="none">
        <circle cx="9" cy="21" r="2" fill="#ccc"/>
        <circle cx="20" cy="21" r="2" fill="#ccc"/>
        <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#ccc" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <div class="agent-cart-empty-text">购物车是空的</div>
      <button class="agent-cart-back-btn" @click="page='agentShop'">去选购</button>
    </div>

    <!-- 备注 -->
    <div class="agent-remark-row" v-if="cart.length > 0">
      <span class="agent-remark-label">备注</span>
      <input class="agent-remark-input" v-model="agentRemark" placeholder="选填，不超过40字" maxlength="40" />
    </div>

    <!-- 底部结算栏 -->
    <div class="agent-bottom-bar" v-if="cart.length > 0">
      <div class="agent-bottom-left">
        <div class="agent-bottom-total">
          <span class="agent-bottom-label">合计</span>
          <span class="agent-bottom-amount">¥{{ cartTotalPrice }}</span>
        </div>
      </div>
      <div class="agent-bottom-pay" @click="confirmAgentOrder">生成订单</div>
    </div>
  </div>

  <!-- ====== 录单 - 模式选择页 ====== -->
  <div class="record-mode-page" v-if="page === 'recordMode'">
    <div class="record-mode-curve"></div>
    <div class="record-mode-header">
      <div class="record-mode-back" @click="page='home'">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
          <path d="M15 18l-6-6 6-6" stroke="rgba(255,255,255,0.7)" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div class="record-mode-brand">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
          <path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8l-6-6z" stroke="rgba(255,255,255,0.9)" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M14 2v6h6" stroke="rgba(255,255,255,0.9)" stroke-width="1.5" stroke-linejoin="round"/>
          <path d="M16 13H8M16 17H8M10 9H8" stroke="rgba(255,255,255,0.9)" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span class="record-mode-title">录单</span>
      </div>
      <div class="record-mode-subtitle">选择录单方式</div>
    </div>

    <div class="record-mode-cards">
      <div class="record-mode-card" @click="startRecordByGoods">
        <div class="record-card-top">
          <div class="record-card-icon goods-icon">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none">
              <circle cx="9" cy="21" r="2" fill="#1565C0"/>
              <circle cx="20" cy="21" r="2" fill="#1565C0"/>
              <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#1565C0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <div class="record-card-title">按商品录单</div>
          <div class="record-card-desc">选择商品录入订单明细</div>
        </div>
        <div class="record-card-bottom">
          <span class="record-card-action">开始录单</span>
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
            <path d="M9 18l6-6-6-6" stroke="#1565C0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
      </div>
      <div class="record-mode-card" @click="startRecordByAmount">
        <div class="record-card-top">
          <div class="record-card-icon amount-icon">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none">
              <rect x="2" y="5" width="20" height="14" rx="3" stroke="#1565C0" stroke-width="1.8"/>
              <circle cx="12" cy="12" r="3" stroke="#1565C0" stroke-width="1.8"/>
              <path d="M7 5V3M17 5V3" stroke="#1565C0" stroke-width="1.8" stroke-linecap="round"/>
            </svg>
          </div>
          <div class="record-card-title">按金额录单</div>
          <div class="record-card-desc">输入实收金额 + 拍照凭证</div>
        </div>
        <div class="record-card-bottom">
          <span class="record-card-action">开始录单</span>
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
            <path d="M9 18l6-6-6-6" stroke="#1565C0" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
      </div>
    </div>

    <div class="record-mode-footer">— 线下收款录入系统 —</div>
  </div>

  <!-- ====== 录单 - 按商品录单（复用代客商品页，但底部按钮不同） ====== -->
  <div class="agent-page" v-if="page === 'recordGoods'">
    <div class="agent-header">
      <div class="agent-top">
        <div class="agent-top-left" @click="page='recordMode'">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M15 18l-6-6 6-6" stroke="#fff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span>返回</span>
        </div>
        <span class="agent-top-title">花漾花店</span>
        <div class="agent-top-cart" @click="goRecordCart">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <circle cx="9" cy="21" r="1.5" fill="#fff"/>
            <circle cx="20" cy="21" r="1.5" fill="#fff"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="agent-cart-badge" v-if="recordCartTotal > 0">{{ recordCartTotal }}</span>
        </div>
      </div>
    </div>

    <!-- 分类标签栏 -->
    <div class="agent-tabs">
      <div class="agent-tab" v-for="(cat, ci) in categories" :key="ci" :class="{active: recordActiveCat === ci}" @click="recordActiveCat = ci">
        {{ cat.name }}
      </div>
    </div>

    <!-- 商品列表 -->
    <div class="agent-products">
      <div class="agent-product" v-for="(prod, pi) in recordProducts" :key="pi" @click="addRecordCart(prod)">
        <div class="agent-prod-img">
          <div class="agent-prod-placeholder">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none">
              <rect x="2" y="2" width="20" height="20" rx="3" stroke="#ccc" stroke-width="1.2"/>
              <path d="M21 15l-5-5-9 9" stroke="#ccc" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
        <div class="agent-prod-info">
          <div class="agent-prod-name">{{ prod.name }}</div>
          <div class="agent-prod-spec" v-if="prod.spec">{{ prod.spec }}</div>
          <div class="agent-prod-price">
            <span class="agent-prod-price-symbol">¥</span>
            <span class="agent-prod-price-val">{{ prod.price }}</span>
          </div>
        </div>
        <div class="agent-prod-add">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <circle cx="12" cy="12" r="10" fill="#42A5F5"/>
            <path d="M12 7v10M7 12h10" stroke="#fff" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
      </div>
    </div>

    <!-- 底部购物车栏 -->
    <div class="agent-bottom-bar" v-if="recordCartTotal > 0" @click="goRecordCart">
      <div class="agent-bottom-left">
        <div class="agent-bottom-cart-icon">
          <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
            <circle cx="9" cy="21" r="2" fill="#42A5F5"/>
            <circle cx="20" cy="21" r="2" fill="#42A5F5"/>
            <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#42A5F5" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="agent-bottom-badge">{{ recordCartTotal }}</span>
        </div>
        <div class="agent-bottom-total">
          <span class="agent-bottom-label">合计</span>
          <span class="agent-bottom-amount">¥{{ recordCartTotalPrice }}</span>
        </div>
      </div>
      <div class="agent-bottom-pay" style="background:linear-gradient(135deg,#1565C0,#42A5F5);box-shadow:0 2px 8px rgba(21,101,192,0.3);" @click.stop="goRecordCart">去结算</div>
    </div>
  </div>

  <!-- ====== 录单 - 按商品 - 购物车确认 ====== -->
  <div class="agent-page" v-if="page === 'recordCart'">
    <div class="agent-header">
      <div class="agent-top">
        <div class="agent-top-left" @click="page='recordGoods'">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M15 18l-6-6 6-6" stroke="#fff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span>返回</span>
        </div>
        <span class="agent-top-title">录入商品</span>
        <div class="agent-top-clear" @click="clearRecordCart" v-if="recordCart.length > 0">清空</div>
      </div>
    </div>

    <div class="agent-cart-list" v-if="recordCart.length > 0">
      <div class="agent-cart-item" v-for="(item, ci) in recordCart" :key="ci">
        <div class="agent-cart-item-left">
          <div class="agent-cart-name">{{ item.name }}</div>
          <div class="agent-cart-spec" v-if="item.spec">{{ item.spec }}</div>
        </div>
        <div class="agent-cart-item-right">
          <div class="agent-cart-price">¥{{ item.price }}</div>
          <div class="agent-cart-qty">
            <button class="agent-qty-btn" @click="recordCartQty(ci, -1)">−</button>
            <span class="agent-qty-val">{{ item.qty }}</span>
            <button class="agent-qty-btn" @click="recordCartQty(ci, 1)">+</button>
          </div>
        </div>
      </div>
    </div>
    <div class="agent-cart-empty" v-else>
      <svg width="48" height="48" viewBox="0 0 24 24" fill="none">
        <circle cx="9" cy="21" r="2" fill="#ccc"/>
        <circle cx="20" cy="21" r="2" fill="#ccc"/>
        <path d="M1 1h4l2.68 13.39a2 2 0 002 1.61h9.72a2 2 0 002-1.61L23 6H6" stroke="#ccc" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <div class="agent-cart-empty-text">未选择商品</div>
      <button class="agent-cart-back-btn" style="background:#42A5F5" @click="page='recordGoods'">去选择</button>
    </div>

    <!-- 备注 -->
    <div class="agent-remark-row" v-if="recordCart.length > 0">
      <span class="agent-remark-label">备注</span>
      <input class="agent-remark-input" v-model="recordCartRemark" placeholder="选填，不超过40字" maxlength="40" />
    </div>

    <!-- 底部结算栏 -->
    <div class="agent-bottom-bar" v-if="recordCart.length > 0">
      <div class="agent-bottom-left">
        <div class="agent-bottom-total">
          <span class="agent-bottom-label">合计</span>
          <span class="agent-bottom-amount">¥{{ recordCartTotalPrice }}</span>
        </div>
      </div>
      <div class="agent-bottom-pay" style="background:linear-gradient(135deg,#1565C0,#42A5F5);box-shadow:0 2px 8px rgba(21,101,192,0.3);" @click="confirmRecordGoods">确认录单</div>
    </div>
  </div>

  <!-- ====== 录单 - 按金额录单（复用商户收款样式，无二维码） ====== -->
  <div class="union-page" v-if="page === 'recordAmount'">
    <div class="union-top-card">
      <div class="union-merchant">
        <img src="/logo.png" class="union-merchant-logo" />
        <span class="union-merchant-name">花漾花店</span>
      </div>
    </div>

    <div class="union-amount-block">
      <div class="union-amount-label">实收金额</div>
      <div class="union-amount-display">
        <span class="union-currency">¥</span>
        <span class="union-amount-text">{{ recordAmountStr || '0.00' }}</span>
      </div>
    </div>

    <div class="union-images-toggle" @click="showRecordImages = !showRecordImages">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <rect x="2" y="2" width="20" height="20" rx="3" stroke="#1565C0" stroke-width="1.5"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#1565C0"/>
        <path d="M21 15l-5-5-9 9" stroke="#1565C0" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#1565C0" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <span>拍照上传凭证 ({{ recordImages.length }}/10)</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" :class="{'arrow-up': showRecordImages}">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="union-images" v-if="showRecordImages">
      <div class="union-img-item" v-for="(img, idx) in recordImages" :key="idx">
        <img :src="img" />
        <div class="union-img-remove" @click="recordImages.splice(idx, 1)">×</div>
      </div>
      <label class="union-img-add" v-if="recordImages.length < 10">
        <input type="file" accept="image/*" capture="environment" @change="onRecordFileChange" style="display:none" />
        <svg width="24" height="24" viewBox="0 0 32 32" fill="none">
          <path d="M16 6v20M6 16h20" stroke="#C8C9CC" stroke-width="2.5" stroke-linecap="round"/>
        </svg>
      </label>
    </div>

    <div class="union-remark-inline" @click="showRecordRemark=true">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <path d="M16 2v4a2 2 0 002 2h4" stroke="#1565C0" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M18 22H6a2 2 0 01-2-2V4a2 2 0 012-2h7l7 7v11a2 2 0 01-2 2z" stroke="#1565C0" stroke-width="1.5" stroke-linejoin="round"/>
      </svg>
      <span class="union-remark-inline-text" v-if="recordAmountRemark">{{ recordAmountRemark }}</span>
      <span class="union-remark-inline-placeholder" v-else>备注</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>

    <div class="union-numpad">
      <div class="union-numpad-keys">
        <button class="union-key" v-for="n in 9" :key="n" @click="recordNumpadInput(String(n))">{{ n }}</button>
        <button class="union-key union-key-zero" colspan="2" @click="recordNumpadInput('0')">0</button>
        <button class="union-key union-key-dot" @click="recordNumpadDot">.</button>
      </div>
      <div class="union-numpad-actions">
        <button class="union-del-btn" @click="recordNumpadDel">×</button>
        <button class="union-confirm-btn" style="background:linear-gradient(135deg,#1565C0,#42A5F5)" @click="confirmRecordAmount" type="button">
          录 单
        </button>
      </div>
    </div>
    <!-- 备注弹窗 -->
    <div class="union-overlay" v-if="showRecordRemark" @click.self="showRecordRemark=false">
      <div class="union-remark-dialog">
        <div class="union-remark-dialog-title">备注</div>
        <textarea class="union-remark-dialog-input" v-model="recordAmountRemark" placeholder="不超过40字" maxlength="40" rows="3"></textarea>
        <div class="union-remark-dialog-actions">
          <button class="union-remark-dialog-cancel" @click="showRecordRemark=false">取消</button>
          <button class="union-remark-dialog-confirm" @click="showRecordRemark=false">确 定</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== 录单成功页 ====== -->
  <div class="success-page" v-if="page === 'recordSuccess'">
    <div class="success-curve"></div>
    <div class="success-status">
      <div class="success-circle">
        <svg width="48" height="48" viewBox="0 0 48 48" fill="none">
          <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.2)"/>
          <path d="M15 24l6 6 12-12" stroke="#fff" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div class="success-title">录单成功</div>
    </div>
    <div class="success-merchant">花漾花店</div>
    <div class="success-amount-bar">
      <span class="success-amount">¥{{ recordSuccessAmount }}</span>
    </div>
    <div class="success-section">
      <div class="success-info-row">
        <span class="info-label">录单金额</span>
        <span class="info-value">¥{{ recordSuccessAmount }}</span>
      </div>
      <div class="success-info-row">
        <span class="info-label">{{ recordSuccessType === 'goods' ? '商品明细' : '付款方式' }}</span>
        <span class="info-value" style="font-size:12px">{{ recordSuccessDetail }}</span>
      </div>
      <div class="success-info-row">
        <span class="info-label">录单时间</span>
        <span class="info-value">{{ recordSuccessTime }}</span>
      </div>
      <div class="success-info-row" v-if="recordSuccessRemark">
        <span class="info-label">备注</span>
        <span class="info-value">{{ recordSuccessRemark }}</span>
      </div>
    </div>
    <div class="success-bottom">
      <button class="back-btn" @click="page='home'">返回首页</button>
    </div>
  </div>

  <!-- ====== 代客下单 - 订单确认/二维码页 ====== -->
  <div class="order-page" v-if="page === 'agentOrderConfirm'">
    <div class="order-curve"></div>
    <div class="order-status">
      <div class="order-status-icon">
        <svg width="40" height="40" viewBox="0 0 48 48" fill="none">
          <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.2)"/>
          <path d="M14 24l7 7 13-13" stroke="#fff" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div class="order-status-title">订单已创建</div>
      <div class="order-status-hint">请出示二维码让顾客扫码支付</div>
    </div>

    <div class="order-merchant-name">花漾花店</div>

    <div class="order-amount-bar">
      <span class="order-amount">¥{{ agentTotalPrice }}</span>
    </div>

    <div class="order-qr-section">
      <div class="order-qr-wrapper qr-overlay-wrap">
        <img :src="agentQRCodeUrl" class="order-qr" />
        <div class="qr-paid-overlay" v-if="showPaidOverlay">
          <div class="qr-paid-icon">
            <svg width="36" height="36" viewBox="0 0 48 48" fill="none">
              <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.25)"/>
              <path d="M14 24l7 7 13-13" stroke="#fff" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <div class="qr-paid-text">已支付</div>
        </div>
      </div>
    </div>
    <div class="order-pay-methods">
      <div class="order-pay-methods-title">支持以下支付方式</div>
      <div class="order-pay-methods-row">
        <div class="order-pay-method">
          <img src="/wechat-pay.png" class="order-pay-icon" />
          <span class="order-pay-name">微信支付</span>
        </div>
        <div class="order-pay-divider"></div>
        <div class="order-pay-method">
          <img src="/alipay.png" class="order-pay-icon" />
          <span class="order-pay-name">支付宝</span>
        </div>
        <div class="order-pay-divider"></div>
        <div class="order-pay-method">
          <img src="/unionpay.png" class="order-pay-icon" />
          <span class="order-pay-name">云闪付</span>
        </div>
      </div>
    </div>

    <div class="order-section" style="margin-top:16px">
      <div class="order-section-title">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
          <path d="M16 2v4a2 2 0 002 2h4" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M18 22H6a2 2 0 01-2-2V4a2 2 0 012-2h7l7 7v11a2 2 0 01-2 2z" stroke="#2E7D52" stroke-width="1.5" stroke-linejoin="round"/>
        </svg>
        商品明细
      </div>
      <div class="order-info-row" v-for="(item, oi) in cart" :key="oi">
        <span class="info-label">{{ item.name }}<span v-if="item.spec"> ({{ item.spec }})</span> × {{ item.qty }}</span>
        <span class="info-value">¥{{ (item.price * item.qty).toFixed(2) }}</span>
      </div>
      <div class="order-info-row" style="border-top: 1px solid #eef2ee; padding-top: 10px; margin-top: 4px;">
        <span class="info-label" style="font-weight:600;color:#333">合计</span>
        <span class="info-value" style="color:#1B5E3C;font-size:16px">¥{{ agentTotalPrice }}</span>
      </div>
      <div class="order-info-row" v-if="agentRemark">
        <span class="info-label">备注</span>
        <span class="info-value">{{ agentRemark }}</span>
      </div>
      <div class="order-info-row">
        <span class="info-label">订单编号</span>
        <span class="info-value order-id">{{ agentOrderId }}</span>
      </div>
      <div class="order-info-row">
        <span class="info-label">创建时间</span>
        <span class="info-value">{{ agentOrderTime }}</span>
      </div>
    </div>

    <div class="order-bottom">
      <button class="back-btn" @click="page='home'">返回首页</button>
      <button class="pay-done-btn" @click="goPaySuccessFromAgent">顾客已付款</button>
    </div>
  </div>

  <!-- ====== 商户发起收款页（银联风格） ====== -->
  <div class="union-page" v-if="page === 'merchantPay'">
    <div class="union-top-card">
      <div class="union-merchant">
        <img src="/logo.png" class="union-merchant-logo" />
        <span class="union-merchant-name">花漾花店</span>
      </div>
    </div>

    <!-- 订单金额 -->
    <div class="union-amount-block">
      <div class="union-amount-label">订单金额</div>
      <div class="union-amount-display">
        <span class="union-currency">¥</span>
        <span class="union-amount-text">{{ currentAmountStr || '0.00' }}</span>
      </div>
    </div>

    <!-- 商品图片（折叠） -->
    <div class="union-images-toggle" @click="showMerchantImages = !showMerchantImages">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <rect x="2" y="2" width="20" height="20" rx="3" stroke="#2E7D52" stroke-width="1.5"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
        <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
        <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      <span>拍照上传商品图片 ({{ merchantImages.length }}/10)</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" :class="{'arrow-up': showMerchantImages}">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="union-images" v-if="showMerchantImages">
      <div class="union-img-item" v-for="(img, idx) in merchantImages" :key="idx">
        <img :src="img" />
        <div class="union-img-remove" @click="merchantImages.splice(idx, 1)">×</div>
      </div>
      <label class="union-img-add" v-if="merchantImages.length < 10">
        <input type="file" accept="image/*" capture="environment" @change="onFileChange($event, 'merchant')" style="display:none" />
        <svg width="24" height="24" viewBox="0 0 32 32" fill="none">
          <path d="M16 6v20M6 16h20" stroke="#C8C9CC" stroke-width="2.5" stroke-linecap="round"/>
        </svg>
      </label>
    </div>
    <div class="union-remark-inline" @click="showRemarkDialog=true">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
        <path d="M16 2v4a2 2 0 002 2h4" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M18 22H6a2 2 0 01-2-2V4a2 2 0 012-2h7l7 7v11a2 2 0 01-2 2z" stroke="#2E7D52" stroke-width="1.5" stroke-linejoin="round"/>
        <path d="M12 11v6" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round"/>
        <path d="M9 14h6" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <span class="union-remark-inline-text" v-if="merchantRemark">{{ merchantRemark }}</span>
      <span class="union-remark-inline-placeholder" v-else>备注附言</span>
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" :class="{'arrow-up': false}">
        <path d="M6 9l6 6 6-6" stroke="#999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="deduction-divider"></div>
    <!-- 抵扣方式 -->
    <div class="union-deductions" style="display:none">
      <div class="deduction-row">
        <span class="deduction-label-row">
          <span class="deduction-label">福利金</span>
          <span class="deduction-balance">余额：{{ merchantWelfareBalance }}</span>
        </span>
        <label class="deduction-toggle">
          <input type="checkbox" :checked="merchantDeductions.welfare > 0" @change="merchantDeductions.welfare = $event.target.checked ? merchantWelfareBalance : 0" />
          <span class="toggle-track"><span class="toggle-knob"></span></span>
        </label>
      </div>
      <div class="deduction-row">
        <span class="deduction-label">礼品卡</span>
        <span class="deduction-giftcard" v-if="merchantDeductions.giftCard > 0" @click="openDeductionInput('merchant', 'giftCard')">{{ merchantDeductions.giftCard }}元</span>
        <span class="deduction-add" v-else @click="openDeductionInput('merchant', 'giftCard')">选择</span>
      </div>
    </div>
    <div class="merchant-bar-badge">
      <img src="/wechat-pay.png" class="pay-icon-img" />
      <span class="pay-icon-text">微信支付</span>
      <img src="/alipay.png" class="pay-icon-img" />
      <span class="pay-icon-text">支付宝</span>
      <img src="/unionpay.png" class="pay-icon-img" />
      <span class="pay-icon-text">云闪付</span>
    </div>
    <div class="union-numpad">
      <div class="union-numpad-keys">
        <button class="union-key" v-for="n in 9" :key="n" @click="numpadInput(String(n))">{{ n }}</button>
        <button class="union-key union-key-zero" colspan="2" @click="numpadInput('0')">0</button>
        <button class="union-key union-key-dot" @click="numpadDot">.</button>
      </div>
      <div class="union-numpad-actions">
        <button class="union-del-btn" @click="numpadDel">×</button>
        <button class="union-confirm-btn" @click="confirmMerchantOrder" type="button">
          确 认
        </button>
      </div>
    </div>
  </div>

  <!-- ====== 订单确认页（带付款码） ====== -->
  <div class="order-page" v-if="page === 'orderConfirm'">
    <div class="order-curve"></div>
    <div class="order-status">
      <div class="order-status-icon">
        <svg width="40" height="40" viewBox="0 0 48 48" fill="none">
          <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.2)"/>
          <path d="M14 24l7 7 13-13" stroke="#fff" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div class="order-status-title">订单已创建</div>
      <div class="order-status-hint">请出示付款码给顾客扫码</div>
    </div>

    <div class="order-merchant-name">花漾花店</div>

    <div class="order-amount-bar">
      <span class="order-amount">¥{{ merchantDisplay }}</span>
    </div>

    <div class="order-qr-section">
      <div class="order-qr-wrapper qr-overlay-wrap">
        <img :src="qrCodeDataUrl" class="order-qr" />
        <div class="qr-paid-overlay" v-if="showPaidOverlay">
          <div class="qr-paid-icon">
            <svg width="36" height="36" viewBox="0 0 48 48" fill="none">
              <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.25)"/>
              <path d="M14 24l7 7 13-13" stroke="#fff" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <div class="qr-paid-text">已支付</div>
        </div>
      </div>
    </div>

    <div class="order-pay-methods">
      <div class="order-pay-methods-title">支持以下支付方式</div>
      <div class="order-pay-methods-row">
        <div class="order-pay-method">
          <img src="/wechat-pay.png" class="order-pay-icon" />
          <span class="order-pay-name">微信支付</span>
        </div>
        <div class="order-pay-divider"></div>
        <div class="order-pay-method">
          <img src="/alipay.png" class="order-pay-icon" />
          <span class="order-pay-name">支付宝</span>
        </div>
        <div class="order-pay-divider"></div>
        <div class="order-pay-method">
          <img src="/unionpay.png" class="order-pay-icon" />
          <span class="order-pay-name">云闪付</span>
        </div>
      </div>
    </div>
    <div class="order-section">
      <div class="order-info-row">
        <span class="info-label">交易金额</span>
        <span class="info-value">¥{{ merchantDisplay }}</span>
      </div>
      <div class="order-info-row">
        <span class="info-label">订单编号</span>
        <span class="info-value order-id">{{ orderId }}</span>
      </div>
      <div class="order-info-row">
        <span class="info-label">创建时间</span>
        <span class="info-value">{{ orderTime }}</span>
      </div>
      <div class="order-info-row" v-if="merchantRemark">
        <span class="info-label">备注</span>
        <span class="info-value">{{ merchantRemark }}</span>
      </div>
    </div>

    <div class="order-bottom">
      <button class="back-btn" @click="page='home'">返回首页</button>
      <button class="pay-done-btn" @click="goPaySuccessFromMerchant">顾客已付款</button>
    </div>
  </div>

  <!-- ====== 支付成功页（原有，现在由用户扫码跳转模拟） ====== -->
  <div class="success-page" v-if="page === 'paySuccess'">
    <div class="success-curve"></div>

    <div class="success-status">
      <div class="success-circle">
        <svg width="48" height="48" viewBox="0 0 48 48" fill="none">
          <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.2)"/>
          <path d="M15 24l6 6 12-12" stroke="#fff" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div class="success-title">支付成功</div>
    </div>

    <div class="success-merchant">花漾花店</div>

    <div class="success-amount-bar">
      <span class="success-amount">¥{{ successAmount }}</span>
    </div>

    <div class="success-section">
      <div class="success-section-title">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
          <rect x="2" y="2" width="20" height="20" rx="3" stroke="#2E7D52" stroke-width="1.5"/>
        <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
        <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <circle cx="8.5" cy="8.5" r="1.5" fill="#2E7D52"/>
          <path d="M21 15l-5-5-9 9" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M15 21l5-5" stroke="#2E7D52" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
        商品图片
      </div>
      <div class="success-images">
        <div class="success-img-item" v-for="(img, idx) in successImages" :key="idx" @click="openPreview(idx)">
          <img :src="img" />
        </div>
      </div>
    </div>

    <div class="success-section">
      <div class="success-info-row">
        <span class="info-label">交易金额</span>
        <span class="info-value">¥{{ successAmount }}</span>
      </div>
      <div class="success-info-row">
        <span class="info-label">下单时间</span>
        <span class="info-value">{{ orderTime }}</span>
      </div>
      <div class="success-info-row">
        <span class="info-label">支付时间</span>
        <span class="info-value">{{ payTime }}</span>
      </div>
      <div class="success-info-row" v-if="successRemark">
        <span class="info-label">备注</span>
        <span class="info-value">{{ successRemark }}</span>
      </div>
    </div>

    <div class="success-bottom">
      <button class="back-btn" @click="page='home'">返回首页</button>
    </div>
  </div>

  <!-- ====== 图片预览弹窗 ====== -->
  <div class="preview-overlay" v-if="previewShow" @click="closePreview">
    <div class="preview-close" @click.stop="closePreview">
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none">
        <circle cx="12" cy="12" r="10" fill="rgba(0,0,0,0.4)"/>
        <path d="M9 9l6 6M15 9l-6 6" stroke="#fff" stroke-width="2" stroke-linecap="round"/>
      </svg>
    </div>
    <div class="preview-counter">{{ previewIndex + 1 }} / {{ previewImages.length }}</div>
    <div class="preview-arrow preview-arrow-left" v-if="previewIndex > 0" @click.stop="prevImg">
      <svg width="36" height="36" viewBox="0 0 24 24" fill="none">
        <circle cx="12" cy="12" r="11" fill="rgba(0,0,0,0.3)"/>
        <path d="M14 8l-4 4 4 4" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="preview-arrow preview-arrow-right" v-if="previewIndex < previewImages.length - 1" @click.stop="nextImg">
      <svg width="36" height="36" viewBox="0 0 24 24" fill="none">
        <circle cx="12" cy="12" r="11" fill="rgba(0,0,0,0.3)"/>
        <path d="M10 8l4 4-4 4" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <div class="preview-img-wrapper" @touchstart.stop="onTouchStart" @touchend.stop="onTouchEnd">
      <img :src="previewImages[previewIndex]" class="preview-img" @click.stop />
    </div>
  </div>

  <!-- 备注弹窗 -->
  <div class="union-overlay" v-if="showRemarkDialog" @click.self="showRemarkDialog=false">
    <div class="union-remark-dialog">
      <div class="union-remark-dialog-title">付款附言</div>
      <textarea class="union-remark-dialog-input" v-model="remark" placeholder="商户可见，不超过40字" maxlength="40" rows="3"></textarea>
      <div class="union-remark-dialog-actions">
        <button class="union-remark-dialog-cancel" @click="showRemarkDialog=false">取消</button>
        <button class="union-remark-dialog-confirm" @click="showRemarkDialog=false">确 定</button>
      </div>
    </div>
  </div>

  <!-- 抵扣金额输入弹窗 -->
  <div class="union-overlay" v-if="showDeductionDialog" @click.self="showDeductionDialog=false">
    <div class="union-remark-dialog">
      <div class="union-remark-dialog-title">
        {{ deductionTarget.type === 'points' ? '积分' : deductionTarget.type === 'welfare' ? '福利金' : '礼品卡' }}抵扣
      </div>
      <input class="deduction-input" type="number" v-model="deductionInputValue" placeholder="输入抵扣金额" min="0" step="0.01" />
      <div class="union-remark-dialog-actions">
        <button class="union-remark-dialog-cancel" @click="showDeductionDialog=false">取消</button>
        <button class="union-remark-dialog-confirm" @click="confirmDeduction">确 定</button>
      </div>
    </div>
  </div>

</template>

<script setup>
import { ref, computed } from 'vue'
import QRCode from 'qrcode'

// ====== 页面路由 ======
const page = ref('home')
const showPaidOverlay = ref(false)

// ====== 用户扫码付款页数据 ======
const amount = ref('')
const remark = ref('')
const imageList = ref([])
const userDeductions = ref({ points: 0, welfare: 0, giftCard: 0 })
const welfareBalance = 6992.15

const displayAmount = computed(() => {
  if (!amount.value) return '0.00'
  const val = parseFloat(amount.value)
  if (isNaN(val)) return '0.00'
  return val.toFixed(2)
})

// ====== 商户发起收款数据 ======
const merchantAmount = ref('')
const merchantRemark = ref('')
const merchantImages = ref([])
const merchantDeductions = ref({ points: 0, welfare: 0, giftCard: 0 })
const merchantWelfareBalance = 6992.15

const merchantDisplay = computed(() => {
  if (!merchantAmount.value) return '0.00'
  const val = parseFloat(merchantAmount.value)
  if (isNaN(val)) return '0.00'
  return val.toFixed(2)
})

// ====== 自定义数字键盘（银联模式，固定在底部） ======
const showUserImages = ref(true)
const showMerchantImages = ref(true)
const showRemarkDialog = ref(false)
const showDeductionDialog = ref(false)
const deductionTarget = ref({ type: 'points', owner: 'user' })
const deductionInputValue = ref('')

function openDeductionInput(owner, type) {
  deductionTarget.value = { owner, type }
  const d = owner === 'user' ? userDeductions.value : merchantDeductions.value
  deductionInputValue.value = d[type] > 0 ? String(d[type]) : ''
  showDeductionDialog.value = true
}

function confirmDeduction() {
  const val = parseFloat(deductionInputValue.value)
  const t = deductionTarget.value
  if (isNaN(val) || val <= 0) {
    // 关闭=清零
    if (t.owner === 'user') userDeductions.value[t.type] = 0
    else merchantDeductions.value[t.type] = 0
  } else {
    if (t.owner === 'user') userDeductions.value[t.type] = val
    else merchantDeductions.value[t.type] = val
  }
  showDeductionDialog.value = false
}

function goUserPay() {
  page.value = 'userPay'
  amount.value = ''
  showUserImages.value = true
}

function goMerchantCollect() {
  page.value = 'merchantPay'
  merchantAmount.value = ''
  showMerchantImages.value = true
}

// 键盘输入：根据当前页面修改对应的金额变量
function getCurrentAmountRef() {
  if (page.value === 'userPay') return { val: amount, set: (s) => { amount.value = s } }
  if (page.value === 'merchantPay') return { val: merchantAmount, set: (s) => { merchantAmount.value = s } }
  return null
}

const currentAmountStr = computed(() => {
  const r = getCurrentAmountRef()
  if (!r) return '0.00'
  const s = r.val.value
  if (!s) return '0.00'
  const n = parseFloat(s)
  if (isNaN(n)) return '0.00'
  return n.toFixed(2)
})

function numpadInput(n) {
  const r = getCurrentAmountRef()
  if (!r) return
  let s = r.val.value ? String(r.val.value) : ''
  // 最多2位小数
  if (s.includes('.') && s.split('.')[1].length >= 2) return
  // 去掉前导0（但保留 0.x）
  if (s === '0' && n !== '.') { s = '' }
  const news = s + n
  r.set(news)
}

function numpadDot() {
  const r = getCurrentAmountRef()
  if (!r) return
  let s = r.val.value ? String(r.val.value) : ''
  if (!s.includes('.')) {
    r.set(s ? s + '.' : '0.')
  }
}

function numpadDel() {
  const r = getCurrentAmountRef()
  if (!r) return
  let s = r.val.value ? String(r.val.value) : ''
  r.set(s.slice(0, -1))
}

function handlePay() {
  if (imageList.value.length === 0) {
    alert('请至少上传一张商品图片')
    return
  }
  const val = parseFloat(amount.value)
  if (isNaN(val) || val <= 0) {
    alert('请输入正确的金额')
    return
  }
  const now = formatTime()
  successAmount.value = val.toFixed(2)
  successImages.value = [...imageList.value]
  successRemark.value = remark.value || ''
  orderTime.value = now
  payTime.value = now
  page.value = 'paySuccess'
}

function resetPage() {
  page.value = 'home'
  amount.value = ''
  remark.value = ''
  imageList.value = []
  userDeductions.value = { points: 0, welfare: 0, giftCard: 0 }
  merchantDeductions.value = { points: 0, welfare: 0, giftCard: 0 }
  merchantAmount.value = ''
  merchantRemark.value = ''
  merchantImages.value = []
  successImages.value = []
  successRemark.value = ''
}



// ====== 代客下单 - 商品数据 ======
const categories = [
  { name: '热销推荐' },
  { name: '鲜花束' },
  { name: '花篮' },
  { name: '绿植盆栽' },
  { name: '永生花' },
  { name: '婚礼用花' },
];

const allProducts = [
  // 热销推荐
  { cat: 0, name: '红玫瑰花束', spec: '33枝 / 礼盒装', price: 199 },
  { cat: 0, name: '向日葵混搭', spec: '大号 / 纸包', price: 168 },
  { cat: 0, name: '粉色康乃馨', spec: '19枝 / 手提花篮', price: 128 },
  { cat: 0, name: '百合花束', spec: '5枝多头 / 透明纸', price: 158 },
  // 鲜花束
  { cat: 1, name: '红玫瑰花束', spec: '33枝 / 礼盒装', price: 199 },
  { cat: 1, name: '粉玫瑰花束', spec: '19枝 / 纸包装', price: 168 },
  { cat: 1, name: '向日葵混搭', spec: '大号 / 纸包', price: 168 },
  { cat: 1, name: '百合花束', spec: '5枝多头 / 透明纸', price: 158 },
  { cat: 1, name: '混搭花束', spec: '随机搭配 / 中号', price: 138 },
  // 花篮
  { cat: 2, name: '开业花篮', spec: '大号 / 高1.2m', price: 388 },
  { cat: 2, name: '开业花篮', spec: '中号 / 高0.8m', price: 268 },
  { cat: 2, name: '探病花篮', spec: '混搭 / 小号', price: 158 },
  { cat: 2, name: '水果花篮', spec: '精美礼篮装', price: 238 },
  // 绿植盆栽
  { cat: 3, name: '发财树', spec: '1.2m / 陶瓷盆', price: 168 },
  { cat: 3, name: '绿萝', spec: '大盆 / 挂式', price: 48 },
  { cat: 3, name: '多肉组合', spec: '10种 / 迷你盆', price: 68 },
  { cat: 3, name: '富贵竹', spec: '10枝 / 玻璃瓶', price: 38 },
  // 永生花
  { cat: 4, name: '永生玫瑰礼盒', spec: '单枝 / 玻璃罩', price: 298 },
  { cat: 4, name: '永生花小熊', spec: '中号 / 礼盒', price: 398 },
  { cat: 4, name: '永生花相框', spec: '6寸 / 立式', price: 228 },
  // 婚礼用花
  { cat: 5, name: '手捧花', spec: '圆形 / 白色系', price: 368 },
  { cat: 5, name: '胸花套装', spec: '6朵 / 礼盒', price: 128 },
  { cat: 5, name: '婚车装饰', spec: '主花+点缀', price: 688 },
];

const activeCategory = ref(0)
const cart = ref([])
const agentRemark = ref('')
const agentOrderId = ref('')
const agentOrderTime = ref('')
const agentQRCodeUrl = ref('')

const currentProducts = computed(() => {
  return allProducts.filter(p => p.cat === activeCategory.value)
})

const cartTotal = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.qty, 0)
})

const cartTotalPrice = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.price * item.qty, 0).toFixed(2)
})

const agentTotalPrice = computed(() => {
  return cartTotalPrice.value
})

function goAgentOrder() {
  page.value = 'agentShop'
  activeCategory.value = 0
}

function goAgentCart() {
  page.value = 'agentCart'
}

function addToCart(prod) {
  const existing = cart.value.find(c => c.name === prod.name && c.spec === prod.spec)
  if (existing) {
    existing.qty++
  } else {
    cart.value.push({ ...prod, qty: 1 })
  }
}

function cartQty(idx, delta) {
  const item = cart.value[idx]
  if (!item) return
  item.qty += delta
  if (item.qty <= 0) {
    cart.value.splice(idx, 1)
  }
}

function clearCart() {
  cart.value = []
  agentRemark.value = ''
}

async function confirmAgentOrder() {
  if (cart.value.length === 0) {
    alert('购物车为空')
    return
  }
  agentOrderId.value = 'HH' + Date.now().toString(36).toUpperCase()
  agentOrderTime.value = formatTime()
  const data = JSON.stringify({
    orderId: agentOrderId.value,
    items: cart.value.map(c => ({ name: c.name, spec: c.spec, price: c.price, qty: c.qty })),
    total: cartTotalPrice.value,
    merchant: '花漾花店',
    remark: agentRemark.value || '',
    ts: Date.now()
  })
  try {
    agentQRCodeUrl.value = await QRCode.toDataURL(data, {
      width: 280,
      margin: 2,
      color: { dark: '#1B5E3C', light: '#ffffff' }
    })
  } catch (e) {
    console.error('QR生成失败', e)
  }
  page.value = 'agentOrderConfirm'
}

// ====== 录单逻辑 ======
const recordCart = ref([])
const recordCartRemark = ref('')
const recordActiveCat = ref(0)
const recordAmountValue = ref('')
const recordImages = ref([])
const showRecordImages = ref(true)
const recordAmountRemark = ref('')
const showRecordRemark = ref(false)
const recordSuccessAmount = ref('0.00')
const recordSuccessType = ref('goods')
const recordSuccessDetail = ref('')
const recordSuccessTime = ref('')
const recordSuccessRemark = ref('')

const recordProducts = computed(() => {
  return allProducts.filter(p => p.cat === recordActiveCat.value)
})

const recordCartTotal = computed(() => {
  return recordCart.value.reduce((sum, item) => sum + item.qty, 0)
})

const recordCartTotalPrice = computed(() => {
  return recordCart.value.reduce((sum, item) => sum + item.price * item.qty, 0).toFixed(2)
})

const recordAmountStr = computed(() => {
  if (!recordAmountValue.value) return '0.00'
  const n = parseFloat(recordAmountValue.value)
  if (isNaN(n)) return '0.00'
  return n.toFixed(2)
})

function goRecord() {
  page.value = 'recordMode'
}

function startRecordByGoods() {
  recordCart.value = []
  recordCartRemark.value = ''
  recordActiveCat.value = 0
  page.value = 'recordGoods'
}

function startRecordByAmount() {
  recordAmountValue.value = ''
  showRecordImages.value = true
  recordImages.value = []
  recordAmountRemark.value = ''
  page.value = 'recordAmount'
}

function goRecordCart() {
  page.value = 'recordCart'
}

function addRecordCart(prod) {
  const existing = recordCart.value.find(c => c.name === prod.name && c.spec === prod.spec)
  if (existing) {
    existing.qty++
  } else {
    recordCart.value.push({ ...prod, qty: 1 })
  }
}

function recordCartQty(idx, delta) {
  const item = recordCart.value[idx]
  if (!item) return
  item.qty += delta
  if (item.qty <= 0) {
    recordCart.value.splice(idx, 1)
  }
}

function clearRecordCart() {
  recordCart.value = []
  recordCartRemark.value = ''
}

function confirmRecordGoods() {
  if (recordCart.value.length === 0) {
    alert('请选择商品')
    return
  }
  recordSuccessType.value = 'goods'
  recordSuccessAmount.value = recordCartTotalPrice.value
  recordSuccessDetail.value = recordCart.value.map(c => `${c.name}×${c.qty}`).join('、')
  recordSuccessRemark.value = recordCartRemark.value || ''
  recordSuccessTime.value = formatTime()
  page.value = 'recordSuccess'
  // 清空购物车
  recordCart.value = []
  recordCartRemark.value = ''
}

function onRecordFileChange(e) {
  const files = Array.from(e.target.files || [])
  if (files.length === 0) return
  const remain = 10 - recordImages.value.length
  const selected = files.slice(0, remain)
  const promises = selected.map(file => {
    return new Promise((resolve) => {
      const reader = new FileReader()
      reader.onload = (ev) => resolve(ev.target.result)
      reader.onerror = () => resolve(null)
      reader.readAsDataURL(file)
    })
  })
  Promise.all(promises).then(results => {
    const valid = results.filter(r => r !== null)
    recordImages.value = [...recordImages.value, ...valid]
  })
}

function recordNumpadInput(n) {
  let s = recordAmountValue.value ? String(recordAmountValue.value) : ''
  if (s.includes('.') && s.split('.')[1].length >= 2) return
  if (s === '0' && n !== '.') { s = '' }
  recordAmountValue.value = s + n
}

function recordNumpadDot() {
  let s = recordAmountValue.value ? String(recordAmountValue.value) : ''
  if (!s.includes('.')) {
    recordAmountValue.value = s ? s + '.' : '0.'
  }
}

function recordNumpadDel() {
  recordAmountValue.value = String(recordAmountValue.value || '').slice(0, -1)
}

function confirmRecordAmount() {
  const val = parseFloat(recordAmountValue.value)
  if (isNaN(val) || val <= 0) {
    alert('请输入正确的金额')
    return
  }
  recordSuccessType.value = 'amount'
  recordSuccessAmount.value = val.toFixed(2)
  recordSuccessDetail.value = '线下收款'
  recordSuccessRemark.value = recordAmountRemark.value || ''
  recordSuccessTime.value = formatTime()
  page.value = 'recordSuccess'
}

function goPaySuccessFromAgent() {
  showPaidOverlay.value = true
  setTimeout(() => {
    const now = formatTime()
    successAmount.value = cartTotalPrice.value
    const desc = cart.value.map(c => `${c.name}×${c.qty}`).join('、')
    successRemark.value = agentRemark.value || desc
    orderTime.value = agentOrderTime.value
    payTime.value = now
    page.value = 'paySuccess'
  }, 1500)
}

// ====== 图片上传 ======
function onFileChange(e, type) {
  const files = Array.from(e.target.files || [])
  if (files.length === 0) return
  const target = type === 'user' ? imageList : merchantImages
  const remain = 10 - target.value.length
  const selected = files.slice(0, remain)
  const promises = selected.map(file => {
    return new Promise((resolve) => {
      const reader = new FileReader()
      reader.onload = (ev) => resolve(ev.target.result)
      reader.onerror = () => resolve(null)
      reader.readAsDataURL(file)
    })
  })
  Promise.all(promises).then(results => {
    const valid = results.filter(r => r !== null)
    target.value = [...target.value, ...valid]
  })
}

function removeImage(idx) {
  imageList.value.splice(idx, 1)
}

// ====== 商户确认收款 ======
const orderId = ref('')

function confirmMerchantOrder() {
  if (merchantImages.value.length === 0) {
    alert('请至少上传一张商品图片')
    return
  }
  const val = parseFloat(merchantAmount.value)
  if (isNaN(val) || val <= 0) {
    alert('请输入正确的金额')
    return
  }
  orderId.value = 'HH' + Date.now().toString(36).toUpperCase()
  orderTime.value = formatTime()
  generateQR()
  page.value = 'orderConfirm'
}

function goPaySuccessFromMerchant() {
  showPaidOverlay.value = true
  setTimeout(() => {
    const now = formatTime()
    successAmount.value = parseFloat(merchantAmount.value).toFixed(2)
    successImages.value = [...merchantImages.value]
    successRemark.value = merchantRemark.value || ''
    orderTime.value = now
    payTime.value = now
    page.value = 'paySuccess'
  }, 1500)
}

// ====== 二维码生成 ======
const qrCodeDataUrl = ref('')

async function generateQR() {
  const data = JSON.stringify({
    orderId: orderId.value,
    amount: merchantAmount.value,
    merchant: '花漾花店',
    ts: Date.now()
  })
  try {
    qrCodeDataUrl.value = await QRCode.toDataURL(data, {
      width: 280,
      margin: 2,
      color: { dark: '#1B5E3C', light: '#ffffff' }
    })
  } catch (e) {
    console.error('QR生成失败', e)
  }
}

// ====== 支付成功页数据 ======
const successAmount = ref('0.00')
const successImages = ref([])
const successRemark = ref('')
const orderTime = ref('')
const payTime = ref('')

function formatTime() {
  const now = new Date()
  const pad = n => String(n).padStart(2, '0')
  return `${now.getFullYear()}-${pad(now.getMonth()+1)}-${pad(now.getDate())} ${pad(now.getHours())}:${pad(now.getMinutes())}:${pad(now.getSeconds())}`
}

// ====== 图片预览 ======
const previewShow = ref(false)
const previewIndex = ref(0)
const previewImages = ref([])
const touchStartX = ref(0)

function openPreview(idx) {
  // 从当前页面获取图片源
  if (page.value === 'orderConfirm') {
    previewImages.value = merchantImages.value
  } else if (page.value === 'paySuccess') {
    previewImages.value = successImages.value
  }
  previewIndex.value = idx
  previewShow.value = true
}

function closePreview() {
  previewShow.value = false
}

function prevImg() {
  if (previewIndex.value > 0) previewIndex.value--
}

function nextImg() {
  if (previewIndex.value < previewImages.value.length - 1) previewIndex.value++
}

function onTouchStart(e) {
  touchStartX.value = e.touches[0].clientX
}

function onTouchEnd(e) {
  const diff = e.changedTouches[0].clientX - touchStartX.value
  if (diff > 50) prevImg()
  else if (diff < -50) nextImg()
}
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
button { touch-action: manipulation; }
body {
  font-family: -apple-system, BlinkMacSystemFont, 'PingFang SC', 'Helvetica Neue', sans-serif;
  background: #f0f2f0;
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;
}

/* ====== 首页 ====== */
.home-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #fff;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 70px;
}
.home-curve {
  position: absolute;
  top: -200px;
  left: -20%;
  width: 140%;
  height: 450px;
  background: linear-gradient(135deg, #1B5E3C 0%, #2E7D52 50%, #4CAF79 100%);
  border-radius: 0 0 50% 50%;
}
.home-brand {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 14px;
  margin-bottom: 4px;
}
.home-logo {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  border: 2px solid rgba(255,255,255,0.3);
}
.home-title {
  font-size: 26px;
  font-weight: 800;
  color: #fff;
  letter-spacing: 4px;
}
.home-subtitle {
  position: relative;
  z-index: 1;
  font-size: 14px;
  color: rgba(255,255,255,0.8);
  margin-bottom: 60px;
  letter-spacing: 2px;
}
.home-btns {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 280px;
}
.home-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  height: 56px;
  border: none;
  border-radius: 28px;
  font-size: 17px;
  font-weight: 600;
  cursor: pointer;
  letter-spacing: 2px;
  transition: transform 0.15s, opacity 0.2s;
}
.home-btn:active {
  transform: scale(0.97);
}
.home-btn-user {
  background: linear-gradient(135deg, #1B5E3C, #2E7D52);
  color: #fff;
  box-shadow: 0 4px 16px rgba(46, 125, 82, 0.35);
}
.home-btn-merchant {
  background: #fff;
  color: #1B5E3C;
  border: 2px solid #2E7D52;
  box-shadow: 0 2px 10px rgba(0,0,0,0.04);
}
.home-footer-text {
  position: absolute;
  bottom: 40px;
  font-size: 13px;
  color: #c8d6c8;
  letter-spacing: 3px;
}
.home-btn-agent {
  background: #fff;
  color: #B8860B;
  border: 2px solid #DAA520;
  box-shadow: 0 2px 10px rgba(0,0,0,0.04);
}
.home-btn-record {
  background: #fff;
  color: #1565C0;
  border: 2px solid #42A5F5;
  box-shadow: 0 2px 10px rgba(0,0,0,0.04);
}

/* ====== 支付页 + 商户收款页 ====== */
.payment-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #fff;
  position: relative;
  padding-bottom: 90px;
  overflow: hidden;
}
.top-curve {
  position: absolute;
  top: -60px;
  left: -20%;
  width: 140%;
  height: 180px;
  background: linear-gradient(135deg, #1B5E3C 0%, #2E7D52 50%, #4CAF79 100%);
  border-radius: 0 0 50% 50%;
}
.header {
  position: relative;
  z-index: 1;
  padding: 12px 20px 8px;
  display: flex;
  align-items: center;
  gap: 12px;
}
.header-back {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.header-title {
  font-size: 17px;
  font-weight: 600;
  color: #fff;
}
.merchant-section {
  position: relative;
  z-index: 1;
  padding: 0 20px;
  margin-top: 4px;
}
.merchant-card {
  display: flex;
  align-items: center;
  gap: 14px;
  background: rgba(255,255,255,0.92);
  backdrop-filter: blur(8px);
  padding: 14px 16px;
  border-radius: 16px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.06);
}
.merchant-avatar img {
  width: 52px;
  height: 52px;
  border-radius: 12px;
  object-fit: contain;
  background: #fff;
  border: 1px solid #e8ece8;
}
.merchant-name {
  font-size: 17px;
  font-weight: 700;
  color: #1a1a1a;
}
.merchant-hint {
  font-size: 12px;
  color: #8e9a8e;
  margin-top: 3px;
}
.amount-section {
  padding: 16px 20px 0;
  position: relative;
  z-index: 2;
}
.amount-card {
  background: linear-gradient(135deg, #F8FBF8, #F0F5F0);
  border: 1px solid #e4ebe4;
  border-radius: 20px;
  padding: 14px 20px 12px;
  text-align: center;
  position: relative;
  overflow: hidden;
}
.amount-label {
  font-size: 13px;
  color: #6b7a6b;
  font-weight: 500;
  letter-spacing: 1px;
  margin-bottom: 6px;
}
.amount-value {
  display: flex;
  align-items: baseline;
  justify-content: center;
  gap: 3px;
}
.currency {
  font-size: 28px;
  color: #1B5E3C;
  font-weight: 800;
  margin-right: 2px;
}
.amount-display {
  font-size: 42px;
  font-weight: 800;
  color: #1B5E3C;
  cursor: pointer;
  min-width: 100px;
  display: inline-block;
}
.amount-input {
  font-size: 42px;
  font-weight: 800;
  color: #1B5E3C;
  border: none;
  outline: none;
  background: transparent;
  width: 200px;
  text-align: center;
  letter-spacing: -1px;
}
.amount-input::-webkit-outer-spin-button,
.amount-input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.amount-input[type=number] {
  -moz-appearance: textfield;
}
.amount-decoration,
.dot {
  display: none;
}
.expand-section {
  padding: 16px 20px 0;
}
.field-row {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px 16px;
  background: #f8faf8;
  border-radius: 14px;
  margin-bottom: 12px;
}
.field-icon {
  display: flex;
  align-items: center;
  flex-shrink: 0;
}
.field-content {
  flex: 1;
}
.field-input {
  width: 100%;
  border: none;
  outline: none;
  font-size: 14px;
  color: #333;
  background: transparent;
  padding: 2px 0;
}
.field-input::placeholder {
  color: #a8b8a8;
}
.upload-area {
  padding: 14px 16px;
  background: #f8faf8;
  border-radius: 14px;
  margin-bottom: 12px;
}
.upload-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
}
.upload-title {
  font-size: 14px;
  font-weight: 600;
  color: #333;
}
.upload-required {
  font-size: 10px;
  color: #fff;
  background: #e53935;
  padding: 1px 6px;
  border-radius: 4px;
  font-weight: 500;
}
.upload-limit {
  font-size: 11px;
  color: #a8b8a8;
  margin-left: auto;
}
.upload-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
.upload-item {
  width: 72px;
  height: 72px;
  border-radius: 12px;
  overflow: hidden;
  position: relative;
  border: 1px solid #e4ebe4;
  background: #fff;
}
.upload-preview {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.upload-remove {
  position: absolute;
  top: -5px;
  right: -5px;
  width: 20px;
  height: 20px;
  background: rgba(0,0,0,0.55);
  color: #fff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  cursor: pointer;
  line-height: 1;
}
.upload-add {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f0f4f0;
  border: 1.5px dashed #d0dbd0;
  cursor: pointer;
  transition: border-color 0.2s;
}
.upload-add:active {
  border-color: #2E7D52;
}
.pay-method-area {
  margin-bottom: 12px;
}
.pay-method-title {
  font-size: 13px;
  color: #6b7a6b;
  font-weight: 500;
  letter-spacing: 1px;
  margin-bottom: 10px;
}
.pay-method-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 16px;
  background: #f8faf8;
  border-radius: 14px;
  border: 1px solid #e4ebe4;
}
.pay-method-left {
  display: flex;
  align-items: center;
  gap: 12px;
}
.wechat-icon {
  display: flex;
  align-items: center;
}
.pay-method-name {
  font-size: 14px;
  font-weight: 600;
  color: #1a1a1a;
}
.pay-method-desc {
  font-size: 11px;
  color: #a8b8a8;
  margin-top: 2px;
}
.pay-method-checked {
  display: flex;
  align-items: center;
}
.bottom-bar {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 420px;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 20px;
  padding-bottom: calc(14px + env(safe-area-inset-bottom, 12px));
  border-top: 1px solid #eaece8;
  box-shadow: 0 -2px 12px rgba(0,0,0,0.04);
}
.bottom-left {
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.bottom-amount-label {
  font-size: 11px;
  color: #8e9a8e;
}
.bottom-amount {
  font-size: 14px;
  color: #1a1a1a;
  font-weight: 700;
}
.amount-big {
  font-size: 24px;
  font-weight: 800;
}
.amount-decimal {
  font-size: 16px;
  font-weight: 600;
  color: #6b7a6b;
}
.pay-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 130px;
  height: 48px;
  border: none;
  border-radius: 24px;
  background: linear-gradient(135deg, #1B5E3C, #2E7D52, #4CAF79);
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  letter-spacing: 1px;
  transition: opacity 0.2s, transform 0.15s;
  box-shadow: 0 4px 14px rgba(46, 125, 82, 0.35);
}
.pay-btn:active {
  opacity: 0.9;
  transform: scale(0.97);
}

/* ====== 订单确认页 ====== */
.order-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #fff;
  position: relative;
  padding: 0 20px 100px;
  overflow: hidden;
}
.order-curve {
  position: absolute;
  top: -220px;
  left: -10%;
  width: 120%;
  height: 500px;
  background: linear-gradient(135deg, #1B5E3C 0%, #2E7D52 50%, #4CAF79 100%);
  border-radius: 0 0 30% 30%;
  box-shadow: inset 0 -8px 20px rgba(0,0,0,0.15);
}
.order-status {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 30px;
}
.order-status-icon {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: rgba(255,255,255,0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
}
.order-status-title {
  font-size: 18px;
  font-weight: 700;
  color: #fff;
  letter-spacing: 2px;
}
.order-status-hint {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  margin-top: 4px;
  letter-spacing: 1px;
}
.order-merchant-name {
  position: relative;
  z-index: 2;
  text-align: center;
  font-size: 14px;
  color: rgba(255,255,255,0.85);
  margin-top: 2px;
}
.order-amount-bar {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 4px 0 12px;
}
.order-amount {
  font-size: 32px;
  font-weight: 800;
  color: #fff;
  letter-spacing: 1px;
}
.order-qr-section {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 8px;
}
.order-qr-wrapper {
  background: #fff;
  padding: 12px;
  border-radius: 14px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.1);
}
.order-qr {
  width: 200px;
  height: 200px;
  display: block;
}
.order-qr-tip {
  font-size: 13px;
  color: #8e9a8e;
  margin-top: 12px;
}
.order-qr-amount {
  font-size: 12px;
  color: #a8b8a8;
  margin-top: 4px;
}
.order-section {
  background: #f8faf8;
  border-radius: 16px;
  padding: 18px;
  margin-bottom: 12px;
}
.order-section-title {
  font-size: 14px;
  font-weight: 600;
  color: #333;
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
}
.order-images {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.order-img-item {
  width: 68px;
  height: 68px;
  border-radius: 10px;
  overflow: hidden;
  border: 1px solid #e4ebe4;
  cursor: pointer;
}
.order-img-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.order-info-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}
.order-info-row + .order-info-row {
  border-top: 1px solid #eef2ee;
}
.info-label {
  font-size: 13px;
  color: #8e9a8e;
}
.info-value {
  font-size: 14px;
  color: #333;
  font-weight: 600;
}
.order-id {
  font-size: 12px;
  color: #666;
  font-weight: 500;
}
.order-bottom {
  text-align: center;
  padding-top: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.pay-done-btn {
  width: 100%;
  height: 48px;
  border: none;
  border-radius: 24px;
  background: linear-gradient(135deg, #1B5E3C, #2E7D52);
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  letter-spacing: 1px;
}
.pay-done-btn:active {
  opacity: 0.85;
  transform: scale(0.97);
}
.back-btn {
  width: 100%;
  height: 48px;
  border: 2px solid #2E7D52;
  border-radius: 24px;
  background: #fff;
  color: #2E7D52;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  letter-spacing: 1px;
  transition: all 0.2s;
}
.back-btn:active {
  background: #f0f8f0;
  transform: scale(0.97);
}

/* ====== 支付成功页面 ====== */
.success-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #fff;
  position: relative;
  padding: 0 20px 100px;
  overflow: hidden;
}
.success-curve {
  position: absolute;
  top: -220px;
  left: -10%;
  width: 120%;
  height: 500px;
  background: linear-gradient(135deg, #1B5E3C 0%, #2E7D52 50%, #4CAF79 100%);
  border-radius: 0 0 30% 30%;
  box-shadow: inset 0 -8px 20px rgba(0,0,0,0.15);
}
.success-status {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 50px;
}
.success-circle {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: rgba(255,255,255,0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16px;
}
.success-title {
  font-size: 22px;
  font-weight: 700;
  color: #fff;
  letter-spacing: 2px;
}
.success-merchant {
  position: relative;
  z-index: 2;
  text-align: center;
  font-size: 15px;
  color: rgba(255,255,255,0.85);
  margin-top: 4px;
}
.success-amount-bar {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 10px 0 50px;
}
.success-amount {
  font-size: 36px;
  font-weight: 800;
  color: #fff;
  letter-spacing: 1px;
}
.success-section {
  background: #f8faf8;
  border-radius: 16px;
  padding: 18px;
  margin-bottom: 12px;
}
.success-section-title {
  font-size: 14px;
  font-weight: 600;
  color: #333;
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
}
.success-images {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.success-img-item {
  width: 68px;
  height: 68px;
  border-radius: 10px;
  overflow: hidden;
  border: 1px solid #e4ebe4;
  cursor: pointer;
}
.success-img-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.success-info-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}
.success-info-row + .success-info-row {
  border-top: 1px solid #eef2ee;
}
.success-bottom {
  text-align: center;
  padding-top: 10px;
}

/* ====== 抵扣方式（竖排列表） ====== */
.deduction-divider {
  height: 6px;
  background: #f5f6f8;
}
.union-deductions {
  padding: 2px 20px 6px;
  background: #fff;
  border-bottom: 1px solid #eee;
}
.deduction-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 4px;
  border-bottom: 0.5px solid #f0f0f0;
}
.deduction-row:last-child { border-bottom: none; }
.deduction-label { font-size: 14px; color: #333; font-weight: 500; }
.deduction-val {
  font-size: 15px;
  color: #1B5E3C;
  font-weight: 700;
  cursor: pointer;
  touch-action: manipulation;
}
.deduction-add {
  font-size: 13px;
  color: #1B5E3C;
  font-weight: 600;
  padding: 5px 14px;
  border-radius: 14px;
  border: 1.5px dashed #1B5E3C;
  cursor: pointer;
  touch-action: manipulation;
  background: #f0fff4;
}
.deduction-label-row {
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.deduction-balance {
  font-size: 11px;
  color: #999;
}
.deduction-toggle {
  display: inline-flex;
  align-items: center;
  cursor: pointer;
}
.deduction-toggle input { display: none; }
.toggle-track {
  width: 44px;
  height: 24px;
  background: #ddd;
  border-radius: 12px;
  position: relative;
  transition: background .2s;
}
.deduction-toggle input:checked + .toggle-track {
  background: #1B5E3C;
}
.toggle-knob {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 20px;
  height: 20px;
  background: #fff;
  border-radius: 50%;
  transition: left .2s;
  box-shadow: 0 1px 3px rgba(0,0,0,.15);
}
.deduction-toggle input:checked + .toggle-track .toggle-knob {
  left: 22px;
}
.deduction-giftcard {
  font-size: 13px;
  color: #1B5E3C;
  font-weight: 600;
  cursor: pointer;
  touch-action: manipulation;
}
.deduction-input {
  width: 100%;
  padding: 12px;
  font-size: 18px;
  border: 1.5px solid #ddd;
  border-radius: 10px;
  outline: none;
  text-align: center;
  font-weight: 600;
  color: #333;
}
.deduction-input:focus {
  border-color: #1B5E3C;
}
/* ====== 图片预览弹窗 ====== */
.preview-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.92);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
}
.preview-close {
  position: fixed;
  top: 50px;
  right: 20px;
  z-index: 10000;
  cursor: pointer;
}
.preview-counter {
  position: fixed;
  top: 56px;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255,255,255,0.6);
  font-size: 14px;
  z-index: 10000;
  font-weight: 500;
}
.preview-arrow {
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
  z-index: 10000;
  cursor: pointer;
}
.preview-arrow-left {
  left: 12px;
}
.preview-arrow-right {
  right: 12px;
}
.preview-img-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}
.preview-img {
  max-width: 95%;
  max-height: 85%;
  object-fit: contain;
  border-radius: 8px;
  user-select: none;
  -webkit-user-drag: none;
}

/* ====== 录单模式选择页 ====== */
.record-mode-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #f5f6f8;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 70px;
}
.record-mode-curve {
  position: absolute;
  top: -200px;
  left: -20%;
  width: 140%;
  height: 420px;
  background: linear-gradient(135deg, #0D47A1 0%, #1565C0 50%, #42A5F5 100%);
  border-radius: 0 0 50% 50%;
}
.record-mode-header {
  position: relative;
  z-index: 2;
  width: 100%;
  padding: 0 20px;
  margin-bottom: 20px;
}
.record-mode-back {
  cursor: pointer;
  margin-bottom: 20px;
}
.record-mode-brand {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 4px;
}
.record-mode-title {
  font-size: 28px;
  font-weight: 800;
  color: #fff;
  letter-spacing: 4px;
}
.record-mode-subtitle {
  font-size: 14px;
  color: rgba(255,255,255,0.7);
  letter-spacing: 2px;
  margin-left: 2px;
}
.record-mode-cards {
  position: relative;
  z-index: 2;
  width: 100%;
  padding: 0 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.record-mode-card {
  background: #fff;
  border-radius: 20px;
  padding: 24px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.06);
  cursor: pointer;
  transition: transform 0.15s;
}
.record-mode-card:active {
  transform: scale(0.97);
}
.record-card-top {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 16px;
}
.record-card-icon {
  width: 64px;
  height: 64px;
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 12px;
}
.goods-icon {
  background: #E3F2FD;
}
.amount-icon {
  background: #E8EAF6;
}
.record-card-title {
  font-size: 18px;
  font-weight: 700;
  color: #1a1a1a;
  margin-bottom: 4px;
}
.record-card-desc {
  font-size: 13px;
  color: #999;
}
.record-card-bottom {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding-top: 14px;
  border-top: 1px solid #f0f0f0;
}
.record-card-action {
  font-size: 14px;
  font-weight: 600;
  color: #1565C0;
}
.record-mode-footer {
  position: absolute;
  bottom: 40px;
  font-size: 13px;
  color: #c8d6c8;
  letter-spacing: 3px;
}

/* ====== 代客下单 ====== */
.agent-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #f5f6f8;
  padding-bottom: 80px;
}
.agent-header {
  background: linear-gradient(135deg, #1B5E3C, #2E7D52);
  padding: 14px 16px;
  position: sticky;
  top: 0;
  z-index: 10;
}
.agent-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.agent-top-left {
  display: flex;
  align-items: center;
  gap: 4px;
  color: #fff;
  font-size: 14px;
  cursor: pointer;
}
.agent-top-title {
  font-size: 17px;
  font-weight: 600;
  color: #fff;
}
.agent-top-cart {
  position: relative;
  cursor: pointer;
  padding: 4px;
}
.agent-cart-badge {
  position: absolute;
  top: -4px;
  right: -4px;
  background: #e53935;
  color: #fff;
  font-size: 10px;
  min-width: 16px;
  height: 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
}
.agent-top-clear {
  color: rgba(255,255,255,0.7);
  font-size: 14px;
  cursor: pointer;
}
.agent-tabs {
  display: flex;
  overflow-x: auto;
  background: #fff;
  border-bottom: 1px solid #eee;
  -webkit-overflow-scrolling: touch;
}
.agent-tab {
  flex-shrink: 0;
  padding: 12px 18px;
  font-size: 14px;
  color: #666;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all 0.2s;
}
.agent-tab.active {
  color: #1B5E3C;
  font-weight: 600;
  border-bottom-color: #1B5E3C;
}
.agent-products {
  padding: 10px 12px;
}
.agent-product {
  display: flex;
  align-items: center;
  background: #fff;
  border-radius: 12px;
  padding: 12px;
  margin-bottom: 8px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
  cursor: pointer;
}
.agent-prod-img {
  width: 64px;
  height: 64px;
  border-radius: 8px;
  overflow: hidden;
  background: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  margin-right: 12px;
}
.agent-prod-placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
}
.agent-prod-info {
  flex: 1;
  min-width: 0;
}
.agent-prod-name {
  font-size: 15px;
  font-weight: 600;
  color: #333;
  margin-bottom: 2px;
}
.agent-prod-spec {
  font-size: 12px;
  color: #999;
  margin-bottom: 4px;
}
.agent-prod-price {
  display: flex;
  align-items: baseline;
}
.agent-prod-price-symbol {
  font-size: 12px;
  font-weight: 600;
  color: #e53935;
}
.agent-prod-price-val {
  font-size: 18px;
  font-weight: 700;
  color: #e53935;
}
.agent-prod-add {
  flex-shrink: 0;
  margin-left: 8px;
  cursor: pointer;
}
.agent-bottom-bar {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 420px;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 20px;
  padding-bottom: calc(10px + env(safe-area-inset-bottom, 10px));
  border-top: 1px solid #eee;
  box-shadow: 0 -2px 10px rgba(0,0,0,0.06);
  z-index: 20;
}
.agent-bottom-left {
  display: flex;
  align-items: center;
  gap: 10px;
}
.agent-bottom-cart-icon {
  position: relative;
}
.agent-bottom-badge {
  position: absolute;
  top: -6px;
  right: -8px;
  background: #e53935;
  color: #fff;
  font-size: 10px;
  min-width: 16px;
  height: 16px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
}
.agent-bottom-total {
  display: flex;
  align-items: baseline;
  gap: 4px;
}
.agent-bottom-label {
  font-size: 12px;
  color: #999;
}
.agent-bottom-amount {
  font-size: 18px;
  font-weight: 800;
  color: #e53935;
}
.agent-bottom-pay {
  background: linear-gradient(135deg, #DAA520, #B8860B);
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  padding: 10px 30px;
  border-radius: 22px;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(218, 165, 32, 0.3);
}
.agent-bottom-pay:active {
  opacity: 0.85;
  transform: scale(0.97);
}

/* 购物车列表 */
.agent-cart-list {
  padding: 12px;
}
.agent-cart-item {
  display: flex;
  align-items: center;
  background: #fff;
  border-radius: 12px;
  padding: 14px;
  margin-bottom: 8px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
}
.agent-cart-item-left {
  flex: 1;
}
.agent-cart-name {
  font-size: 15px;
  font-weight: 600;
  color: #333;
}
.agent-cart-spec {
  font-size: 12px;
  color: #999;
  margin-top: 2px;
}
.agent-cart-item-right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 6px;
}
.agent-cart-price {
  font-size: 16px;
  font-weight: 700;
  color: #e53935;
}
.agent-cart-qty {
  display: flex;
  align-items: center;
  gap: 6px;
}
.agent-qty-btn {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: 1.5px solid #ddd;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  color: #666;
  cursor: pointer;
  touch-action: manipulation;
}
.agent-qty-btn:active {
  background: #f0f0f0;
}
.agent-qty-val {
  font-size: 16px;
  font-weight: 600;
  color: #333;
  min-width: 20px;
  text-align: center;
}
.agent-cart-empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding-top: 80px;
  color: #ccc;
}
.agent-cart-empty-text {
  font-size: 14px;
  color: #999;
  margin-top: 12px;
  margin-bottom: 20px;
}
.agent-cart-back-btn {
  background: #DAA520;
  color: #fff;
  border: none;
  padding: 10px 30px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
}
.agent-remark-row {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 16px;
  background: #fff;
  margin: 0 12px;
  border-radius: 10px;
  margin-bottom: 80px;
}
.agent-remark-label {
  font-size: 14px;
  color: #666;
  flex-shrink: 0;
}
.agent-remark-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: 14px;
  color: #333;
}
.agent-remark-input::placeholder {
  color: #bbb;
}

/* ====== 自定义数字键盘 ====== */
/* ====== 银联风格页面 ====== */
.union-page {
  max-width: 420px;
  margin: 0 auto;
  min-height: 100vh;
  background: #f5f6f8;
  display: flex;
  flex-direction: column;
}
.union-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 20px 0;
  background: transparent;
}
.union-header-back {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.union-header-back svg path {
  stroke: #fff;
}
.union-header-title {
  font-size: 17px;
  font-weight: 600;
  color: #fff;
}
.union-top-card {
  background: linear-gradient(135deg, #1B5E3C 0%, #2E7D52 100%);
  padding: 24px 20px 40px;
  border-radius: 0 0 24px 24px;
  margin-bottom: -20px;
}
.union-merchant {
  display: flex;
  align-items: center;
  gap: 14px;
}
.union-merchant-logo {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  object-fit: contain;
  border: 2px solid rgba(255,255,255,0.25);
}
.union-merchant-name {
  font-size: 20px;
  font-weight: 700;
  color: #fff;
}
.union-amount-block {
  background: #fff;
  padding: 14px 16px 18px;
  text-align: center;
}
.union-amount-label {
  font-size: 14px;
  color: #999;
  margin-bottom: 6px;
}
.union-amount-display {
  font-size: 36px;
  font-weight: 800;
  color: #1a1a1a;
  letter-spacing: -0.5px;
}
.union-currency {
  font-size: 24px;
  font-weight: 700;
  color: #1a1a1a;
  margin-right: 4px;
}
.union-amount-text {
  font-size: 40px;
  font-weight: 800;
  color: #1a1a1a;
}
.union-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 16px;
  background: #fff;
  border-bottom: 1px solid #eee;
}
.union-row-label {
  font-size: 15px;
  color: #333;
  font-weight: 500;
}
.union-row-value {
  font-size: 15px;
  color: #1a1a1a;
}
.union-row-placeholder {
  color: #bbb;
}
.union-row-input {
  border: none;
  outline: none;
  font-size: 15px;
  color: #333;
  text-align: right;
  flex: 1;
  max-width: 200px;
  background: transparent;
}
.union-row-input::placeholder {
  color: #bbb;
}
.union-images-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 16px;
  background: #fff;
  cursor: pointer;
  font-size: 14px;
  color: #666;
  border-bottom: 1px solid #eee;
}
.union-images-toggle svg:last-child {
  margin-left: auto;
  transition: transform 0.2s;
}
.union-images-toggle .arrow-up {
  transform: rotate(180deg);
}
.union-remark-display {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 10px 16px;
  font-size: 14px;
  color: #666;
  background: #fff;
  border-top: 1px solid #eee;
}
.union-remark-display svg {
  flex-shrink: 0;
}
.union-remark-label {
  color: #666;
  font-size: 14px;
  flex-shrink: 0;
}
.union-images {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  background: #fff;
  padding: 8px 16px 16px;
}
.union-img-item {
  width: 60px;
  height: 60px;
  border-radius: 6px;
  overflow: hidden;
  position: relative;
  border: 1px solid #eee;
}
.union-img-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.union-img-remove {
  position: absolute;
  top: -4px;
  right: -4px;
  width: 18px;
  height: 18px;
  background: rgba(0,0,0,0.5);
  color: #fff;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  cursor: pointer;
}
.union-img-add {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
  border: 1px dashed #ddd;
  border-radius: 6px;
  cursor: pointer;
}

/* ====== 底部固定键盘 ====== */
.union-numpad {
  display: grid;
  grid-template-columns: 3fr 1fr;
  background: #e0e0e0;
  border-top: 1px solid #d5d5d5;
  margin-top: auto;
}
.union-numpad-keys {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1px;
  background: #d5d5d5;
  border-right: 1px solid #d5d5d5;
}
.union-key {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 56px;
  background: #fff;
  border: none;
  font-size: 22px;
  font-weight: 500;
  color: #333;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}
.union-key:active {
  background: #d5d5d5;
  transform: scale(0.94);
}
.union-key-zero {
  grid-column: span 2;
}
.union-key-dot {
  font-weight: 800;
}
.union-numpad-actions {
  display: flex;
  flex-direction: column;
}
.union-del-btn {
  height: 56px;
  border: none;
  background: #fff;
  color: #999;
  font-size: 28px;
  font-weight: 600;
  cursor: pointer;
  border-bottom: 1px solid #d5d5d5;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}
.union-del-btn:active {
  background: #e8e8e8;
  transform: scale(0.94);
}
.union-confirm-btn {
  flex: 1;
  border: none;
  background: linear-gradient(135deg, #1B5E3C, #2E7D52);
  color: #fff;
  font-size: 20px;
  font-weight: 800;
  letter-spacing: 2px;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
  touch-action: manipulation;
  min-height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.union-confirm-btn:active {
  opacity: 0.85;
  transform: scale(0.94);
}
.union-key.pressed, .union-del-btn.pressed, .union-confirm-btn.pressed {
  transition: none;
}
.union-key.pressed { background: #d5d5d5; transform: scale(0.94); }
.union-del-btn.pressed { background: #e8e8e8; transform: scale(0.94); }
.union-confirm-btn.pressed { opacity: 0.85; transform: scale(0.94); }

/* ====== 备注按钮 & 弹窗 ====== */
.union-remark-inline {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 16px;
  background: #fff;
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  font-size: 14px;
  color: #666;
  border-bottom: 1px solid #eee;
}
.union-remark-inline span {
  color: #666;
}
.union-remark-inline svg:last-child {
  margin-left: auto;
}
.union-remark-inline-text {
  flex: 1;
  color: #333;
}
.union-remark-inline-placeholder {
  flex: 1;
  color: #bbb;
}
.union-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.45);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}
.union-remark-dialog {
  background: #fff;
  border-radius: 16px;
  width: 300px;
  padding: 24px 20px 16px;
}
.union-remark-dialog-title {
  font-size: 16px;
  font-weight: 600;
  color: #1a1a1a;
  text-align: center;
  margin-bottom: 14px;
}
.union-remark-dialog-input {
  width: 100%;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 10px 12px;
  font-size: 14px;
  color: #333;
  outline: none;
  resize: none;
  font-family: inherit;
  background: #f9f9f9;
}
.union-remark-dialog-input::placeholder {
  color: #bbb;
}
.union-remark-dialog-actions {
  display: flex;
  gap: 10px;
  margin-top: 14px;
}
.union-remark-dialog-cancel {
  flex: 1;
  border: 1px solid #ddd;
  background: #fff;
  color: #666;
  font-size: 15px;
  font-weight: 500;
  border-radius: 8px;
  padding: 10px;
  cursor: pointer;
}
.union-remark-dialog-confirm {
  flex: 1;
  border: none;
  background: linear-gradient(135deg, #1B5E3C, #2E7D52);
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  border-radius: 8px;
  padding: 10px;
  cursor: pointer;
}
/* ====== 支付方式标注 ====== */
.pay-method-item {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 12px;
  color: #999;
}
.pay-method-item svg {
  flex-shrink: 0;
  border-radius: 2px;
}
.pay-icon-img {
  width: 18px;
  height: 18px;
  border-radius: 3px;
  flex-shrink: 0;
}
.agent-bar-badge {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
  padding: 8px 0 12px;
  background: transparent;
}
.agent-bar-badge .pay-icon-img {
  margin-right: -2px;
}
.merchant-bar-badge {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 4px;
  padding: 24px 0 14px;
  background: transparent;
  font-size: 13px;
  color: #999;
}
.merchant-bar-badge .pay-icon-img {
  margin: 0 2px;
}
.pay-icon-text {
  margin-right: 10px;
}
.pay-icon-text:last-of-type {
  margin-right: 0;
}
.qr-overlay-wrap {
  position: relative;
}
.qr-paid-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.55);
  border-radius: 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 10;
  animation: fadeIn 0.3s ease;
}
.qr-paid-icon {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: rgba(255,255,255,0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
}
.qr-paid-text {
  font-size: 16px;
  font-weight: 700;
  color: #fff;
  letter-spacing: 4px;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.order-pay-methods {
  padding: 12px 20px 4px;
  background: #fff;
}
.order-pay-methods-title {
  text-align: center;
  font-size: 12px;
  color: #999;
  margin-bottom: 8px;
}
.order-pay-methods-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0;
}
.order-pay-method {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 0 10px;
}
.order-pay-icon {
  width: 22px;
  height: 22px;
  border-radius: 3px;
  flex-shrink: 0;
}
.order-pay-name {
  font-size: 13px;
  color: #333;
  font-weight: 500;
}
.order-pay-divider {
  width: 1px;
  height: 18px;
  background: #e8e8e8;
}
</style>
