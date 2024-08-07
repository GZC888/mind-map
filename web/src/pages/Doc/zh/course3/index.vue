<template>
  <div>
    <h1>插入/删除节点、前进回退</h1>
<p>首先和操作节点内容一样，也需要监听节点的激活事件，然后禁用相关按钮。</p>
<p>以下命令都支持传递一些参数，详情请参考【API】-【构造函数】-【execCommand方法】小节中该命令的介绍。</p>
<h2>插入子节点</h2>
<p>插入子节点很简单，执行<code>INSERT_CHILD_NODE</code>命令即可：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>)
</code></pre>
<p>这样就会在当前激活节点（如果存在多个激活节点，默认会操作第一个激活节点）下添加一个子节点。</p>
<p>如果你想获取插入节点的实例，可以这样操作：</p>
<p>1.需要指定新插入节点的<code>id</code>，比如：</p>
<pre class="hljs"><code><span class="hljs-keyword">import</span> { createUid } <span class="hljs-keyword">from</span> <span class="hljs-string">&#x27;simple-mind-map/src/utils&#x27;</span>

<span class="hljs-keyword">let</span> uid = createUid()
mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>, <span class="hljs-literal">false</span>, [], {
    uid
})
</code></pre>
<p>2.然后在<code>node_tree_render_end</code>事件里通过该<code>id</code>来获取实例：</p>
<pre class="hljs"><code>mindMap.on(<span class="hljs-string">&#x27;node_tree_render_end&#x27;</span>, <span class="hljs-function">() =&gt;</span> {
    <span class="hljs-comment">// 调用renderer实例的findNodeByUid方法获取到节点的实例对象</span>
    <span class="hljs-keyword">const</span> node = mindMap.renderer.findNodeByUid(uid)
})
</code></pre>
<p>插入节点的命令也可以传入一定参数，比如创建新节点不想直接进入新节点的编辑模式，那么可以这样调用：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>, <span class="hljs-literal">false</span>)
</code></pre>
<p>如果想给指定的节点插入新节点，而不是当前激活的节点，那么就可以通过第二个参数：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>, <span class="hljs-literal">false</span>, [node])
</code></pre>
<p>参数是通过平铺的列表方式传递的，所以前面的参数都不能省略。</p>
<p>如果要指定创建的新节点的一些数据，那么可以通过第三个参数：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>, <span class="hljs-literal">false</span>, [], {
    <span class="hljs-attr">uid</span>: <span class="hljs-string">&#x27;指定uid&#x27;</span>,
    <span class="hljs-attr">text</span>: <span class="hljs-string">&#x27;指定初始文本&#x27;</span>
})
</code></pre>
<p>最后一个参数可以指定创建新节点的子节点：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_CHILD_NODE&#x27;</span>, <span class="hljs-literal">false</span>, [], {
    <span class="hljs-attr">uid</span>: <span class="hljs-string">&#x27;指定uid&#x27;</span>,
    <span class="hljs-attr">text</span>: <span class="hljs-string">&#x27;指定初始文本&#x27;</span>
}, [
    {
        <span class="hljs-attr">data</span>: {
            <span class="hljs-attr">text</span>: <span class="hljs-string">&#x27;下级节点&#x27;</span>
        },
        <span class="hljs-attr">children</span>: []
    }
])
</code></pre>
<p>注意传递的是完整的节点结构数据。</p>
<p>其他命令也是类似的，详细可以参考api文档。</p>
<h2>插入多个子节点</h2>
<p>如果你要同时插入多个子节点，那么可以执行<code>INSERT_MULTI_CHILD_NODE</code>命令：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_MULTI_CHILD_NODE&#x27;</span>, [], childList)
</code></pre>
<p><code>childList</code>是要插入的子节点数据的数组，必传。</p>
<pre class="hljs"><code>[
    {
        <span class="hljs-attr">data</span>: {
              <span class="hljs-attr">text</span>: <span class="hljs-string">&#x27;自定义节点1&#x27;</span>
        }
    }
]
</code></pre>
<h2>插入同级节点</h2>
<p>插入同级节点和插入子节点方式完全一致：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_NODE&#x27;</span>)
</code></pre>
<h2>插入多个同级节点</h2>
<p>插入多个同级节点可以执行<code>INSERT_MULTI_NODE</code>命令：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_MULTI_NODE&#x27;</span>. [], nodeList)
</code></pre>
<p><code>nodeList</code>是要插入的同级节点数据的数组，必传。</p>
<h2>插入父节点</h2>
<p>要插入父节点可以调用<code>INSERT_PARENT_NODE</code>命令：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;INSERT_PARENT_NODE&#x27;</span>)
</code></pre>
<h2>删除节点</h2>
<p>删除节点也是执行命令：</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;REMOVE_NODE&#x27;</span>)
</code></pre>
<p>会删除当前激活的所有节点。</p>
<h2>仅删除当前节点</h2>
<p><code>REMOVE_CURRENT_NODE</code>命令可以仅删除激活的节点，子节点不会被删除。</p>
<pre class="hljs"><code>mindMap.execCommand(<span class="hljs-string">&#x27;REMOVE_CURRENT_NODE&#x27;</span>)
</code></pre>
<h2>前进回退</h2>
<p>首先需要监听<code>back_forward</code>事件，事件回调中可以获取当前的历史记录总数，以及当前所在的历史记录索引，那么就可以判断当前是否处于历史记录的最开始，还是最后，然后对前进回退按钮进行禁用。</p>
<pre class="hljs"><code><span class="hljs-keyword">const</span> isStart = ref(<span class="hljs-literal">true</span>)
<span class="hljs-keyword">const</span> isEnd = ref(<span class="hljs-literal">true</span>)
mindMap.on(<span class="hljs-string">&#x27;back_forward&#x27;</span>, <span class="hljs-function">(<span class="hljs-params">index, len</span>) =&gt;</span> {
    isStart.value = index &lt;= <span class="hljs-number">0</span>
    isEnd.value = index &gt;= len - <span class="hljs-number">1</span>
})
</code></pre>
<p>然后前进回退调用相关命令即可：</p>
<pre class="hljs"><code><span class="hljs-comment">// 回退一次</span>
mindMap.execCommand(<span class="hljs-string">&#x27;BACK&#x27;</span>)

<span class="hljs-comment">// 前进一次</span>
mindMap.execCommand(<span class="hljs-string">&#x27;FORWARD&#x27;</span>)
</code></pre>
<h2>完整示例</h2>
<iframe style="width: 100%; height: 455px; border: none;" src="https://wanglin2.github.io/playground/#eNrFVt1qG0cUfpXTLUVykVYy9EqVRBxZpaaxXZTSXmSCWe2OpGl2Z5bdkeRgBCYkkP7RtKWU1tCm0NJetKUXpRCnIS/jtZ236Jmd/dMPWLmKYMXOnHO+78ycM9/OkbHl++ZkTI2G0QztgPkSQirHfptw5vkikHAEAR1UQPBdMeaSOhUIR5brimmPDmAGg0B4UEKEUhaxy7iza/naRIwQp11a9XC26lk+MQgHINylEtSc8mwBH7su4YTXanD519/Rs2+jT764fH4Snfz44viYcFvwUAILb0oLCVoqpbIMxnQjN3W5M29QJAgXPfsGsc6fH5//8/Tih/uXn967uPckevjd5c+/p8GWLdmE7gmHhgiRL6986zbiaJS5RPqWfQc9yxvQasORokpWYtJDaneE51ncKZeub3XeKyHCLAWJ15SCDEQwtQKV9FU47+z3PtrqbRehzr/8Onrwa/TgfvTfT3pJ2U7wkAZSLWYN5J29m93eBwd7+9vdFeh/PloF3Rkx13k5/M67Oze2l1iih49ffP/LPIVDsS/iUqyB3uvu7n/YncfN+rS8HK36jE7T/izHJgDqNsAR9tijXJpDKrsuVa/X7+4gRRLZEVxajNOgtFHRUY4lrYZGVz9iqAliFKb0tKSHUk0T4/zxk2St8QlQv1kCphxtta0B5cr5Vo6xALeSZZHp7PTzi9PfFsnmCVeQ3s5tRb9XlEH6msylcYwz2RMibvD3RcgkExwjSy4dyFIFSjaWDssUR8023tYyAIDtdnHyVfToD52TloOz08/Onv5bbC/ByyWOyAdaEhCwrIYVUP83WCjznoKibJgTyx2rlk39NH3CTmR2+LWOrCZWqnKQqIJiRhM9rIBLeZEVt0DLYMYZ+0GzBfXcA9Vwwd5uKSSowmaam3qaNa36qPc4kBSl2pIURwBNh03Adq0wbBEjyXObeoIYsTlxYE5uzU4JujRraC06pkhSCLdvKRdtJLLZH0spOFyzXWbfQRe1EcSASZUNcPRasl6M0LvXrOmABADxFwGSTZzDwB1RCHEVrkbIVTQHKdYbt3IoR9DGPTfay2K8LkGmpeuzpKJ8NUWupWugF8V4ATovZfrWrBU6BYehvOvqprmW3AGIYdb0hz9RW5OGnmmHITGyM2kWmirt7ilz5KgBm/X6G7EfgJ8d8oAiI2YfG2J9UM/ri82XQuWBVj8U7ljqQMBjMJANqCcjKfx8sEw/omw4Qve36nX/MGVezftmyuxZwZAhb4rqW47D+DCdyFI3k7OwZsabaQZJ0tkYAfEcxzUwKoaugLpqmR+HguPNLobHlosNWIFMvImBFzet2GYNX80AP53Mo6pY1X4gptijCEKMRHxXXOZ07HKpVVSS28yY/Q8ZbqFI" />
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>