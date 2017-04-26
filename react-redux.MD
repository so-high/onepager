# @connect
## connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options

### mapStateToProps(state, [ownProps]): stateProps
- if this argument is specified, the new component will subscribe to Redux store updates
- This means that any time the store is updated, mapStateToProps will be called, and the result will be merged into the component's props 
- if ownProps is specified, its value will be the props passed to your component, and mapStateToProps will be additionally re-invoked whenever the component receives new props

### mapDispatchToProps(dispatch, [ownProps]): dispatchProps(Object or Function)
- if an object is passed, each function inside it is assumed to be a Redux action creator. An object with the same function names but with every acton creator wrapped into a dispatch call so they may be invoked directly, will be merged into the component's props
- If a function is passed, it will be given dispatch	
