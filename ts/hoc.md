```
import React, { ReactNode } from 'react';
import { Action } from 'redux';
import { connect } from 'react-redux';

import { State as AppState } from '../reducers';

/**
 * Abstract loading of a requested entity and selecting from redux state.
 * @param Component The component to wrap with this HoC and provide the requested entity.
 * @param options How to identify, load and select the entity.
 */
export default function connectEntity<
  propname extends string,
  TEntity,
  OuterProps extends object = {},
  InnerProps extends OuterProps &
    TInjectedProps<propname, TEntity> = OuterProps &
    TInjectedProps<propname, TEntity>
>(options: TOptions<propname, TEntity, OuterProps>) {
  return (
    Component: React.ComponentType<InnerProps>,
  ): React.ComponentType<OuterProps> => {
    const mapStateToProps = (state: AppState, props: OuterProps) => {
      const id = options.identifierFromPropsResolver(props);
      const entity = options.entitySelector(state, id);
      const error = options.errorSelector
        ? options.errorSelector(state, id)
        : false;
      const loading = !entity && !error;
      return {
        [options.property]: entity,
        loading,
        error,
      };
    };

    const mapDispatchToProps = options.loadEntityAction
      ? {
          loadEntity: options.loadEntityAction,
        }
      : null;

    type TConnectedWrappedProps = InnerProps & {
      loadEntity?: (id: string) => void;
    };

    class Wrapped extends React.Component<TConnectedWrappedProps> {
      componentDidMount() {
        if (!this.props[options.property] && this.props.loadEntity) {
          const id = options.identifierFromPropsResolver(this.props);
          this.props.loadEntity(id);
        }
      }

      componentDidUpdate(prevProps: TConnectedWrappedProps) {
        if (!this.props.loadEntity) {
          return;
        }

        const prevId = options.identifierFromPropsResolver(prevProps);
        const id = options.identifierFromPropsResolver(this.props);

        if (id !== prevId) {
          this.props.loadEntity(id);
        }
      }

      render() {
        const { loading, error } = this.props;

        if (error && options.renderError) {
          return options.renderError();
        }

        if (loading && options.renderLoading) {
          return options.renderLoading();
        }

        const { loadEntity, ...rest } = this.props;
        return <Component {...rest as InnerProps} />;
      }
    }

    return connect(
      mapStateToProps,
      mapDispatchToProps,
      // @ts-ignore
    )(Wrapped);
  };
}

/**
 * Additional props selected by the HoC to be passed down to the passed component.
 */
type TInjectedProps<propname extends string, TEntity> = {
  loading: boolean;
  error: boolean;
} & { [k in propname]?: TEntity };

/**
 * @template TEntity the type of the entity you expect to find in the redux store
 * @template TOuterProps props passed into the connected component. Most likely react-router props.
 */
export type TOptions<propname extends string, TEntity, TOuterProps> = {
  property: propname;
  identifierFromPropsResolver: <P extends TOuterProps>(props: P) => string;
  loadEntityAction?: (id?: string) => Action;
  entitySelector: (state: AppState, id: string) => TEntity | undefined;
  errorSelector?: (state: AppState, id: string) => boolean | undefined;
  renderError?: () => ReactNode;
  renderLoading?: () => ReactNode;
};
```

