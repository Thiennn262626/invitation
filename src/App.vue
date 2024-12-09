<template>
  <div id="app">
    <!-- Header -->
    <el-header class="header">
      <el-menu
        mode="horizontal"
        :default-active="activeTab"
        @select="handleMenuClick"
        class="menu"
      >
        <el-menu-item index="1">Thư mời</el-menu-item>
        <el-menu-item index="2">Hướng dẫn</el-menu-item>
        <el-menu-item index="3">Cảm ơn</el-menu-item>
      </el-menu>
    </el-header>

    <!-- Body -->
    <el-main class="main">
      <section ref="section1" class="section invitation">
        <el-image
          :src="require('@/assets/a.png')"
          alt="Mô tả hình ảnh"
          fit="cover"
          style="width: 300px; height: 200px"
          @error="handleImageError"
        />
      </section>
      <section ref="section2" class="section guide">
        <el-image
          :src="require('@/assets/b.png')"
          alt="Mô tả hình ảnh"
          fit="cover"
          style="width: 300px; height: 200px"
          @error="handleImageError"
        />
      </section>
      <section ref="section3" class="section thanks">
        <el-image
          :src="imagePath"
          alt="Mô tả hình ảnh"
          fit="cover"
          style="width: 300px; height: 200px"
          @error="handleImageError"
        />
      </section>
    </el-main>
  </div>
</template>

<script>
import ImageSrc from '@/assets/c.png'

export default {
  data() {
    return {
      activeTab: '1', // Tab đang được chọn
      imagePath: ImageSrc, // Đường dẫn hình ảnh mặc định
    }
  },
  methods: {
    handleMenuClick(index) {
      // Scroll đến phần nội dung tương ứng
      const sectionRefs = {
        1: this.$refs.section1,
        2: this.$refs.section2,
        3: this.$refs.section3,
      }
      const target = sectionRefs[index]
      if (target) {
        target.scrollIntoView({ behavior: 'smooth', block: 'start' })
      }
    },
    handleScroll() {
      // Lấy vị trí hiện tại của các phần nội dung
      const sections = [
        { index: '1', element: this.$refs.section1 },
        { index: '2', element: this.$refs.section2 },
        { index: '3', element: this.$refs.section3 },
      ]

      // Xác định phần nội dung nào đang nằm trong vùng nhìn thấy
      const currentSection = sections.find((section) => {
        const rect = section.element.getBoundingClientRect()
        const viewportHeight =
          window.innerHeight || document.documentElement.clientHeight
        return rect.top >= 0 && rect.top <= viewportHeight / 2
      })

      // Cập nhật trạng thái tab nếu cần
      if (currentSection && currentSection.index !== this.activeTab) {
        this.activeTab = currentSection.index
      }
    },
    handleImageError(event) {
      // Đặt hình ảnh mặc định nếu không tải được
      event.target.src = require('@/assets/c.png')
    },
  },
  mounted() {
    // Gắn sự kiện scroll vào phần tử chính
    const mainElement = this.$el.querySelector('.main')
    mainElement.addEventListener('scroll', this.handleScroll)
  },
  beforeUnmount() {
    // Hủy sự kiện scroll khi component bị gỡ bỏ
    const mainElement = this.$el.querySelector('.main')
    mainElement.removeEventListener('scroll', this.handleScroll)
  },
}
</script>

<style>
/* CSS */
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.header {
  background-color: white;
  color: white;
  padding: 0;
}

.menu {
  line-height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.main {
  flex: 1;
  overflow-y: auto;
}

.section {
  height: 95vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-bottom: 1px solid #ddd;
  margin-bottom: 20px;
}

.el-image {
  height: 100%;
}

.invitation {
  background-color: #f0f9ff;
}

.guide {
  background-color: #e6f7ff;
}

.thanks {
  background-color: #f0f5ff;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f0f0f0;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: #409eff;
  border-radius: 4px;
  border: 2px solid #f0f0f0;
  transition: background 0.3s;
}

::-webkit-scrollbar-thumb:hover {
  background: #005bbb;
}
</style>
