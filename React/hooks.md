### Hooks
- - - 
- useContent 
예시 코드 
```javaScript
const gnb = useContent({
    source: 'site-service',
    query: {
      hierarchy: 'Mypage',
    },
    filter: '{ children { url display_name node_type } }',
});
```

- useRef
예시 코드 
```javaScript
  const wrapper = menuTab.current;
  const wrapperRect = wrapper.getBoundingClientRect();
    ...
  <div className='nav__bar-center | flex flex--justify-center flex--align-items-center' ref={menuTab}>
        {makeCenterLinks(gnbData)}
  </div>
```
- useEffect