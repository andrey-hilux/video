 {% include styles.html %}

 {% include github-corner.html url="https://github.com/andrey-hilux/video/blob/main/docs/bugs-upgrade-guide-quasar-v-2-to-v-3.md" %}

# BUGS Upgrade Guide Quasar v1 (Vue 2) to Quasar v2 (Vue 3)

#### *[Back to context](https://github.com/quasarframework/quasar/issues/9705)*

#### All In One Video:

<sup id="git-hub-video-player-1"></sup>

{% include video-player.html url="JTdCJTIyY29uZiUyMiUzQSUyMmh0dHBzJTNBJTJGJTJGYW5kcmV5LWhpbHV4LmdpdGh1Yi5pbyUyRnZpZGVvJTJGdmlkZW8lMkZidWdzLXVwZ3JhZGUtZ3VpZGUtcXVhc2FyLXYtMi10by12LTMlMkZjb25mLmpzb24lMjIlN0Q=" %}

<p class="no-html"><a href="https://andrey-hilux.github.io/video/docs/bugs-upgrade-guide-quasar-v-2-to-v-3.html#git-hub-video-player-1"><img src="/video/bugs-upgrade-guide-quasar-v-2-to-v-3/poster.png" alt="BUGS Upgrade Guide Quasar V2 to V3"></a></p>

> *[the document specified GitHub Video Player](https://andrey-hilux.github.io/github-hls-video-player/#git-hub-hls-video-player-tutorial-video) with "Smart Position" for reload of this page*

#### [Author Correction:](https://github.com/quasarframework/quasar/issues/9731)

As the developer said - "Why lazy loading directly from Quasar's src files when the Quasar App CLI does this anyway. Also not quite ok to specify a "js" file instead of a stylesheet file for quasar.conf/css.". This video can have some mistakes, hence please read the new instruction for this issue:

```js
   // remove quasar.conf.js/framework/cssAddon - It still not work
   // change css in quasar.conf.js to :
   css: [
           'app.sass'
       ],
       // add src\css\app.sass file with:
       @import 'quasar/src/css/flex-addon.sass'
   // in your src\App.vue insert:
   import 'app/src/css/app.styl'
   // to head, or to bottom this:
   <
   style lang = "stylus" >
       @import './css/app.styl'
       ...
```

#### 1. stylus-loader

```
# "stylus-loader": ^6 - vuejs devs still works on it
# only this supported "stylus-loader": "^3.0.2"
 npm i -D stylus-loader@3.0.2 stylus@0.54.8
#in .styl or <style scoped>
::v-deep -> :deep()
>>> -> :deep()
# https://github.com/vuejs/rfcs/blob/master/active-rfcs/0023-scoped-styles-changes.md

```

#### 2. quasar.conf.js/framework/cssAddon

* [! Author Correction click for more](https://andrey-hilux.github.io/video/docs/bugs-upgrade-guide-quasar-v-2-to-v-3.html#author-correction)

```
cssAddon: true // -  only for quasar.conf.js/css: ['app.sass'],
```

##### For others please use:

```
# remove this: quasar.conf.js/framework/cssAddon
# turn in to : quasar.conf.js/css: ['app.js']
```

```js
// in your 'app/src/css/app.js'

Promise.all([
    import('quasar/src/css/index.sass'),
    import('quasar/src/css/flex-addon.sass'),
    import('~@/css/app.styl') // for stylus or any for your webpack loaders
]).then(() => {})
```

#### 3. node_modules/quasar/src/components/scroll-area/QScrollArea.js:ln:349

** when release the Thumb on scroll area **

<img src="https://i.ibb.co/NYHtKpv/215.jpg" />

### Vue3 Bugs:

#### 1. template recursive side-effect

*affirmation:* [Vue registers the change and trigger rendering again](https://stackoverflow.com/questions/65689261/vue-3-number-0-increase-is-not-1-but-101-in-template-why)
- [BEST ANSWER](https://laracasts.com/discuss/channels/vue/what-is-patch-in-vue-dev-tools?reply=716661)
- [good article that gives some performance enhancement pointers](https://betterprogramming.pub/6-ways-to-speed-up-your-vue-js-application-2673a6f1cde4)

<sup id="git-hub-video-player-2"></sup>

{% include video-player.html url="JTdCJTIyY29uZiUyMiUzQSUyMmh0dHBzJTNBJTJGJTJGYW5kcmV5LWhpbHV4LmdpdGh1Yi5pbyUyRnZpZGVvJTJGdmlkZW8lMkZ2dWUtMy1idWdzJTJGdGVtcGxhdGUtcmVjdXJzaXZlLS1zaWRlLWVmZmVjdCUyRmNvbmYuanNvbiUyMiU3RA==" %}

<p class="no-html"><a href="https://andrey-hilux.github.io/video/docs/bugs-upgrade-guide-quasar-v-2-to-v-3.html#git-hub-video-player-2"><img src="https://andrey-hilux.github.io/video/video/vue-3-bugs/template-recursive--side-effect/poster.jpg" alt="Vue 3 BUGS: Template recursive side-effect"></a></p>

#### 2. vue-loader
- node_modules/quasar/src/components/virtual-scroll/QVirtualScroll.js:ln:65
- clue:`_ctx`

<sup id="git-hub-video-player-3"></sup>

{% include video-player.html url="JTdCJTIyY29uZiUyMiUzQSUyMmh0dHBzJTNBJTJGJTJGYW5kcmV5LWhpbHV4LmdpdGh1Yi5pbyUyRnZpZGVvJTJGdmlkZW8lMkZ2dWUtMy1idWdzJTJGdnVlLWxvYWRlci0tX2N0eCUyRmNvbmYuanNvbiUyMiU3RA==" %}

<p class="no-html"><a href="https://andrey-hilux.github.io/video/docs/bugs-upgrade-guide-quasar-v-2-to-v-3.html#git-hub-video-player-3"><img src="https://andrey-hilux.github.io/video/video/vue-3-bugs/vue-loader--_ctx/poster.jpg" alt="Vue 3 BUGS: 'vue-loader' _ctx"></a></p>



### Breaking:

#### 1. extends $option

```js
import {
    QInput
} from 'quasar'

export default {
    extends: QInput,
    // ...
}
```

[dsc 1](https://github.com/quasarframework/quasar/discussions/9735)
[dsc 2](https://github.com/quasarframework/quasar/discussions/8761)

> ! V3 add this:

```js
export default {
    extends: QInput,
    setup(props, setupContext) {
        const renderFn = QInput.setup(props, setupContext)
        // expose public methods
        const vm = getCurrentInstance()
        Object.assign(vm.proxy, {
            // focus,
            // select,
            // getNativeElement: () => inputRef.value
        })
        return renderFn
    },
    // ...
}
```
### Helpers:

#### 1. [Proxy (debug)](https://github.com/quasarframework/quasar/discussions/9736)

*Source:*

```js
const getCircularReplacer = () => {
    const seen = new WeakSet()
    return (key, value) => {
        if (typeof value === 'object' && value !== null) {
            if (seen.has(value)) {
                return
            }
            seen.add(value)
        }
        return value
    }
}

const pto = function(obj) {
    return JSON.parse(JSON.stringify(obj, getCircularReplacer()))
}
export {
    pto
}
```

*Use:*

```js
 created() {
         if (process.env.DEV) {
             window.top._pto = window._pto = require('app/src/utils/js/dev').pto
         }
     }

     ...

     mounted() {
         console.log(window._pto(this))
     },
```
